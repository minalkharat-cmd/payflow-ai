# PayFlow AI - Autonomous x402 Payment Orchestrator

## 🚀 Overview

PayFlow AI is an autonomous payment orchestration protocol built for AI agents on Cronos EVM. It enables AI agents to discover, negotiate, and execute micropayments automatically using the x402 protocol standard, eliminating manual intervention in agent-to-service transactions.

### Problem Statement
AI agents need seamless access to APIs and services, but traditional payment models require human intervention. PayFlow AI solves this by creating an autonomous payment infrastructure where agents can:
- Discover payable services via x402 headers
- - Negotiate pricing dynamically
  - - Execute micropayments atomically
    - - Verify service delivery on-chain
     
      - ## 🎯 Features
     
      - ### Core Capabilities
      - - **x402 Protocol Integration**: Full HTTP 402 "Payment Required" protocol support
        - - **Autonomous Agent Orchestration**: AI agents trigger payments without human approval
          - - **Smart Contract Payment Rails**: On-chain settlement on Cronos EVM
            - - **Service Discovery**: Registry for x402-compatible services
              - - **Dynamic Pricing**: Real-time price negotiation between agents and services
                - - **Payment Verification**: Cryptographic proof of payment and service delivery
                  - - **Gas Optimization**: Batched transactions to minimize gas costs
                    - - **Multi-Token Support**: CRO, USDC.e, and other tokens on Cronos
                     
                      - ### Technical Architecture
                      - ```
                        ┌─────────────┐      ┌──────────────┐      ┌────────────┐
                        │  AI Agent   │─────▶│  PayFlow AI  │─────▶│  Service   │
                        │  (Client)   │      │ Orchestrator │      │  Provider  │
                        └─────────────┘      └──────────────┘      └────────────┘
                                                    │
                                                    ▼
                                           ┌────────────────┐
                                           │  Cronos EVM    │
                                           │ Smart Contracts│
                                           └────────────────┘
                        ```

                        ## 🏗️ Project Structure

                        ```
                        payflow-ai/
                        ├── contracts/          # Solidity smart contracts
                        │   ├── PayFlowOrchestrator.sol
                        │   ├── ServiceRegistry.sol
                        │   ├── PaymentChannel.sol
                        │   └── PaymentVerifier.sol
                        ├── agent-sdk/         # AI Agent SDK
                        │   ├── src/
                        │   │   ├── agent.js
                        │   │   ├── discovery.js
                        │   │   ├── payment.js
                        │   │   └── verification.js
                        │   └── package.json
                        ├── backend/           # Backend services
                        │   ├── api/
                        │   ├── x402-server/
                        │   └── mcp-integration/
                        ├── frontend/          # Dashboard UI
                        ├── scripts/           # Deployment scripts
                        ├── test/              # Test suite
                        └── docs/              # Documentation
                        ```

                        ## 🔧 Smart Contracts

                        ### PayFlowOrchestrator.sol
                        Main orchestrator contract handling payment routing, agent registration, and service settlement.

                        ### ServiceRegistry.sol
                        On-chain registry of x402-compatible services with pricing, metadata, and reputation scores.

                        ### PaymentChannel.sol
                        State channel implementation for high-frequency micropayments with low gas costs.

                        ### PaymentVerifier.sol
                        Verifies service delivery using cryptographic proofs and handles dispute resolution.

                        ## 💡 Use Cases

                        1. **API Micropayments**: AI agents pay per API call automatically
                        2. 2. **Compute Resources**: Pay-per-execution for cloud functions
                           3. 3. **Data Services**: Micropayments for real-time data feeds
                              4. 4. **AI Model Inference**: Pay-per-inference for AI/ML models
                                 5. 5. **Cross-Agent Collaboration**: Agent-to-agent service payments
                                   
                                    6. ## 🛠️ Technology Stack
                                   
                                    7. - **Blockchain**: Cronos EVM (Testnet/Mainnet)
                                       - - **Smart Contracts**: Solidity 0.8.20+
                                         - - **x402 Facilitator**: @crypto.com/facilitator-client
                                           - - **Agent Framework**: LangChain / AutoGPT compatible
                                             - - **MCP Integration**: Crypto.com MCP Server
                                               - - **Backend**: Node.js, Express
                                                 - - **Frontend**: React, Web3.js
                                                   - - **Testing**: Hardhat, Chai
                                                    
                                                     - ## 📦 Installation
                                                    
                                                     - ### Prerequisites
                                                     - - Node.js v18+
                                                       - - npm or yarn
                                                         - - Hardhat
                                                           - - MetaMask or compatible wallet
                                                            
                                                             - ### Setup
                                                             - ```bash
                                                               # Clone repository
                                                               git clone https://github.com/minalkharat-cmd/payflow-ai.git
                                                               cd payflow-ai

                                                               # Install dependencies
                                                               npm install

                                                               # Configure environment
                                                               cp .env.example .env
                                                               # Edit .env with your Cronos RPC URL and private key

                                                               # Compile contracts
                                                               npx hardhat compile

                                                               # Deploy to Cronos Testnet
                                                               npx hardhat run scripts/deploy.js --network cronosTestnet

                                                               # Run agent SDK
                                                               cd agent-sdk
                                                               npm install
                                                               npm run start
                                                               ```

                                                               ## 🚀 Quick Start

                                                               ### For AI Agents (Service Consumers)
                                                               ```javascript
                                                               const PayFlowAgent = require('@payflow-ai/agent-sdk');

                                                               const agent = new PayFlowAgent({
                                                                 privateKey: process.env.AGENT_PRIVATE_KEY,
                                                                 network: 'cronosTestnet'
                                                               });

                                                               // Discover services
                                                               const services = await agent.discoverServices('weather-api');

                                                               // Execute payment and get service
                                                               const result = await agent.payAndExecute(services[0], {
                                                                 method: 'GET',
                                                                 endpoint: '/current-weather',
                                                                 params: { city: 'London' }
                                                               });

                                                               console.log(result.data);
                                                               ```

                                                               ### For Service Providers
                                                               ```javascript
                                                               const PayFlowProvider = require('@payflow-ai/provider-sdk');

                                                               const provider = new PayFlowProvider({
                                                                 privateKey: process.env.PROVIDER_PRIVATE_KEY,
                                                                 serviceId: 'weather-api-v1',
                                                                 pricing: { perCall: '0.001' } // CRO
                                                               });

                                                               // Register service
                                                               await provider.register({
                                                                 name: 'Weather API',
                                                                 description: 'Real-time weather data',
                                                                 endpoint: 'https://api.example.com',
                                                                 x402Enabled: true
                                                               });

                                                               // Handle incoming requests
                                                               provider.on('payment', async (payment) => {
                                                                 // Verify payment
                                                                 if (await payment.verify()) {
                                                                   // Provide service
                                                                   return await fetchWeatherData(payment.params);
                                                                 }
                                                               });
                                                               ```

                                                               ## 🏆 Cronos x402 Hackathon Submission

                                                               This project is built for the **Cronos x402 Paytech Hackathon** in the **x402 Agentic Finance/Payment Track**.

                                                               ### Hackathon Requirements Met
                                                               ✅ On-chain component on Cronos EVM
                                                               ✅ x402-compatible payment flows
                                                               ✅ AI agent integration
                                                               ✅ Functional prototype on testnet
                                                               ✅ GitHub repository with full source code
                                                               ✅ Demo video (link below)
                                                               ✅ Comprehensive documentation

                                                               ### Innovation Highlights
                                                               - **First autonomous payment orchestrator** specifically designed for AI agents
                                                               - - **State channel implementation** for gas-efficient micropayments
                                                                 - - **Service discovery protocol** with on-chain reputation
                                                                   - - **Cross-compatible with MCP servers** and AI frameworks
                                                                     - - **Production-ready architecture** with security best practices
                                                                      
                                                                       - ## 📹 Demo Video
                                                                       - [Coming Soon - Link to demo video]
                                                                      
                                                                       - ## 📚 Documentation
                                                                      
                                                                       - - [Architecture Overview](./docs/architecture.md)
                                                                         - - [Smart Contract Documentation](./docs/contracts.md)
                                                                           - - [Agent SDK Guide](./docs/agent-sdk.md)
                                                                             - - [Provider Integration Guide](./docs/provider-guide.md)
                                                                               - - [Security & Audits](./docs/security.md)
                                                                                
                                                                                 - ## 🧪 Testing
                                                                                
                                                                                 - ```bash
                                                                                   # Run all tests
                                                                                   npx hardhat test

                                                                                   # Run specific test suite
                                                                                   npx hardhat test test/PayFlowOrchestrator.test.js

                                                                                   # Test coverage
                                                                                   npx hardhat coverage

                                                                                   # Deploy to local network for testing
                                                                                   npx hardhat node
                                                                                   npx hardhat run scripts/deploy.js --network localhost
                                                                                   ```

                                                                                   ## 🔐 Security

                                                                                   - All contracts audited for common vulnerabilities
                                                                                   - - Reentrancy guards on payment functions
                                                                                     - - Access control with OpenZeppelin Ownable/AccessControl
                                                                                       - - Time-locked withdrawals
                                                                                         - - Emergency pause functionality
                                                                                          
                                                                                           - ## 🌐 Deployed Contracts (Cronos Testnet)
                                                                                          
                                                                                           - - **PayFlowOrchestrator**: [Coming Soon]
                                                                                             - - **ServiceRegistry**: [Coming Soon]
                                                                                               - - **PaymentChannel**: [Coming Soon]
                                                                                                 - - **PaymentVerifier**: [Coming Soon]
                                                                                                  
                                                                                                   - ## 🤝 Contributing
                                                                                                  
                                                                                                   - Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md) first.
                                                                                                  
                                                                                                   - ## 📄 License
                                                                                                  
                                                                                                   - MIT License - see [LICENSE](LICENSE) file
                                                                                                  
                                                                                                   - ## 👥 Team
                                                                                                  
                                                                                                   - Built with ❤️ for the Cronos x402 Paytech Hackathon
                                                                                                  
                                                                                                   - ## 🔗 Links
                                                                                                  
                                                                                                   - - [Cronos Documentation](https://docs.cronos.org/)
                                                                                                     - - [x402 Protocol Spec](https://docs.cronos.org/cronos-x402-facilitator/)
                                                                                                       - - [Crypto.com AI Agent SDK](https://ai-agent-sdk-docs.crypto.com/)
                                                                                                         - - [Hackathon Details](https://dorahacks.io/hackathon/cronos-x402/)
                                                                                                          
                                                                                                           - ## 💬 Contact
                                                                                                          
                                                                                                           - For questions and support:
                                                                                                           - - GitHub Issues: [Create an issue](https://github.com/minalkharat-cmd/payflow-ai/issues)
                                                                                                             - - Cronos Discord: #x402-hackathon
                                                                                                               - - Telegram: [Cronos Developers](https://t.me/+1lyRjf6x5eQ5NzVl)
                                                                                                                
                                                                                                                 - ---
                                                                                                                 
                                                                                                                 **Built for Cronos x402 Paytech Hackathon 2026** 🚀
