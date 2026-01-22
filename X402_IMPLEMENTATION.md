# PayFlow AI - X402 Protocol Implementation

## X402 Compliance Architecture

This document outlines PayFlow AI's integration with the Cronos X402 Facilitator service, implementing proper HTTP 402 payment flows for AI agent micropayments.

## Core Components

### 1. X402 Facilitator Integration

We use the official Cronos X402 Facilitator service to handle payment verification and settlement:

- Facilitator URL: https://facilitator.cronoslabs.org/v2/x402
- - Network: Cronos Testnet (cronos-testnet)
  - - Token: USDC.e (0xc01efAaF7C5C61bEbFAeb358E1161b537b8bC0e0)
    - - Protocol: X402 v1 with EIP-3009 gasless payments
     
      - ### 2. HTTP 402 Payment Flow
     
      - #### Step 1: Resource Request (402 Response)
      - When AI agents request protected resources without payment, server returns HTTP 402:
     
      - ```
        GET /api/ai/gpt4-completion
        Response: 402 Payment Required
        {
          "error": "Payment Required",
          "x402Version": 1,
          "paymentRequirements": {
            "scheme": "exact",
            "network": "cronos-testnet",
            "payTo": "0xMerchantAddress",
            "asset": "0xc01efAaF7C5C61bEbFAeb358E1161b537b8bC0e0",
            "maxAmountRequired": "1000000",
            "maxTimeoutSeconds": 300
          }
        }
        ```

        #### Step 2: Payment Authorization (EIP-3009)
        Client signs EIP-3009 authorization with EIP-712 typed data:
        - Gasless payment (no native tokens needed)
        - - Unique nonce prevents replay attacks
          - - Time-bounded validity
           
            - #### Step 3: Payment Verification
            - Server verifies payment using Facilitator /verify endpoint:
            - - Validates signature cryptographically
              - - Checks network, asset, amount requirements
                - - No on-chain transaction yet
                 
                  - #### Step 4: Payment Settlement
                  - Server settles payment using Facilitator /settle endpoint:
                  - - Submits transferWithAuthorization to blockchain
                    - - Facilitator pays gas fees
                      - - Returns transaction hash and confirmation
                       
                        - #### Step 5: Resource Delivery
                        - Server grants access with payment proof:
                        - - Creates entitlement with 24-hour validity
                          - - Returns API key and transaction hash
                            - - Client can access resource
                             
                              - ### 3. Pricing Model
                             
                              - AI Model micropayments (USDC.e - 6 decimals):
                              - - GPT-4 Completion: 1 USDC (1000000 base units)
                                - - Claude Completion: 1.5 USDC (1500000 base units)
                                  - - Image Generation: 2 USDC (2000000 base units)
                                    - - Data Analysis: 0.5 USDC (500000 base units)
                                     
                                      - ### 4. Security Features
                                     
                                      - - EIP-3009 signature verification
                                        - - Nonce-based replay protection
                                          - - Time-bounded authorizations
                                            - - Cryptographic payment proofs
                                              - - No private key exposure
                                               
                                                - ### 5. Backend Architecture
                                               
                                                - Express.js server with X402 middleware:
                                                - - @crypto.com/facilitator-client for API calls
                                                  - - In-memory entitlement tracking
                                                    - - CORS-enabled for frontend integration
                                                      - - Environment-based configuration
                                                       
                                                        - ### 6. Key Endpoints
                                                       
                                                        - ```
                                                          GET /health - Health check
                                                          GET /api/models - List available AI models with pricing
                                                          GET /api/ai/:model - Protected resource (returns 402 or data)
                                                          POST /api/pay - Payment submission and settlement
                                                          ```

                                                          ### 7. Deployment Configuration

                                                          Environment Variables:
                                                          - NODE_ENV=production
                                                          - - NETWORK=cronos-testnet
                                                            - - MERCHANT_ADDRESS=<Your Wallet Address>
                                                            - PORT=8787
                                                           
                                                            - ### 8. Dependencies
                                                           
                                                            - ```json
                                                              {
                                                                "express": "^4.18.2",
                                                                "cors": "^2.8.5",
                                                                "@crypto.com/facilitator-client": "latest",
                                                                "dotenv": "^16.3.1"
                                                              }
                                                              ```

                                                              ## Testing on Cronos Testnet

                                                              ### Get Test Tokens
                                                              1. Visit: https://faucet.cronos.org
                                                              2. 2. Request testnet CRO
                                                                 3. 3. Get devUSDC.e from testnet faucet
                                                                   
                                                                    4. ### Test Flow
                                                                    5. 1. Start backend server
                                                                       2. 2. Request protected resource (receive 402)
                                                                          3. 3. Sign EIP-3009 authorization with MetaMask
                                                                             4. 4. Submit payment to /api/pay
                                                                                5. 5. Receive access with transaction hash
                                                                                   6. 6. Verify payment on Cronoscan testnet
                                                                                     
                                                                                      7. ## Differences from Custom Implementation
                                                                                     
                                                                                      8. ### Before (Custom Smart Contract):
                                                                                      9. - Built PayFlowOrchestrator.sol from scratch
                                                                                         - - No standard protocol compliance
                                                                                           - - Required custom wallet integration
                                                                                             - - Manual gas management
                                                                                              
                                                                                               - ### After (X402 Facilitator):
                                                                                               - - Uses official Cronos X402 Facilitator
                                                                                                 - - Standards-compliant HTTP 402 flows
                                                                                                   - - Compatible with x402-enabled wallets
                                                                                                     - - Gasless payments via EIP-3009
                                                                                                       - - Production-ready infrastructure
                                                                                                        
                                                                                                         - ## Compliance Checklist
                                                                                                        
                                                                                                         - - [x] Uses Cronos X402 Facilitator service
                                                                                                           - [ ] - [x] Implements HTTP 402 status codes
                                                                                                           - [ ] - [x] EIP-3009 gasless payments
                                                                                                           - [ ] - [x] Proper payment verification flow
                                                                                                           - [ ] - [x] On-chain settlement
                                                                                                           - [ ] - [x] Deployed to Cronos Testnet
                                                                                                           - [ ] - [x] Transaction proof delivery
                                                                                                           - [x] Standards-based architecture
                                                                                                          
                                                                                                           - [ ] ## Live Deployment
                                                                                                          
                                                                                                           - [ ] Backend Service: https://payflow-ai-backend.onrender.com
                                                                                                           - [ ] Network: Cronos Testnet
                                                                                                           - [ ] Facilitator: https://facilitator.cronoslabs.org/v2/x402
                                                                                                          
                                                                                                           - [ ] Contract Address: 0xc01efAaF7C5C61bEbFAeb358E1161b537b8bC0e0 (USDC.e)
                                                                                                           - [ ] Merchant Address: Will be updated post-deployment
                                                                                                          
                                                                                                           - [ ] ## Demo Video
                                                                                                          
                                                                                                           - [ ] A comprehensive demo video showing the complete X402 payment flow is available demonstrating:
                                                                                                           - [ ] - HTTP 402 response when accessing protected resources
                                                                                                           - [ ] - EIP-3009 signature creation
                                                                                                           - [ ] - Facilitator verification and settlement
                                                                                                           - [ ] - On-chain transaction confirmation
                                                                                                           - [ ] - Resource access grant with payment proof
                                                                                                          
                                                                                                           - [ ] ## Resources
                                                                                                          
                                                                                                           - [ ] - X402 Protocol Spec: https://github.com/coinbase/x402
                                                                                                           - [ ] - Cronos Facilitator Docs: https://docs.cronos.org/cronos-x402-facilitator
                                                                                                           - [ ] - Example Implementation: https://github.com/cronos-labs/x402-examples
                                                                                                           - [ ] - Testnet Explorer: https://testnet.cronoscan.com
                                                                                                          
                                                                                                           - [ ] ## Future Enhancements
                                                                                                          
                                                                                                           - [ ] 1. Frontend client with MetaMask integration
                                                                                                           - [ ] 2. Multi-token support (CRO, other stablecoins)
                                                                                                           - [ ] 3. Subscription-based pricing models
                                                                                                           - [ ] 4. Rate limiting and usage analytics
                                                                                                           - [ ] 5. Database persistence for entitlements
                                                                                                           - [ ] 6. WebSocket real-time payment notifications
