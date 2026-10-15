Meeting Summary
Kong Proxy Onboarding & Architecture Alignment

Purpose
Align on Kong Proxy authentication, development-environment testing, and a profile-based architecture for Amazon and Arsenal integrations.
1. Kong Proxy Onboarding & Authentication
• Kong Proxy role: Applications call a Kong URL, which acts as a proxy and handles mutual TLS (mTLS) and authorization before forwarding requests to Amazon.
• OAuth 2 authentication: The application integration is expected to use OAuth 2 through ID Anywhere, avoiding the need for application code to manage web signatures or mTLS directly.
• Roles and FIDs: The team reviewed existing C2C deployments and role assignments. The Amazon deployment template identifies an “app Amazon ECS role,” provided by the co-brand owners (noted in the discussion as Idaho Delegacy for 700-467).
2. Environment Differences: Dev vs. UAT/Production
• UAT and Production: Applications integrate directly with Kong through the standard onboarding process and defined roles.
• Development challenge: The Dev environment does not have a Kong API deployment, so an alternative is needed to support end-to-end testing and verify REST requests.
• Proposed option — dev controller: Create an Amazon-specific profile for a controller (for example, a “dev controller”) within the same application. Requests can loop back to the application for logging and confirmation, avoiding downstream dependencies.
• Alternative option: Create a separate application or Lambda function under a different seal. This was considered higher overhead than the profile-based approach.
3. Application Architecture & Partner-Specific Configuration
• Profile-based configuration: Use profiles and partner-specific YAML files to separate Amazon and Arsenal implementations cleanly.
• Package structure: Organize configuration code under src/main/java/config into distinct Amazon and Arsenal packages so partner-specific beans and classes are easy to identify.
• Code reuse: Use interfaces and abstract classes (for example, an AbstractEventPublisher implementing or extending the appropriate abstraction) to reduce duplicate delivery-service logic while allowing partner-specific publishers to be wired dynamically.
• Codebase analysis: Review integration points, including how messages are consumed and published to Kafka. Build a tracking list that classifies classes as generic or partner-specific, and consider AI-assisted refactoring.
4. Web Client & OAuth Configuration
• YAML-based setup: Communication with Kong requires an Idaho authorization token. Custom security headers and manual certificate handling are not expected in application code; configuration is handled through Photon framework default YAML properties.
• Web client bean processing: Photon automatically wires OAuth configuration by matching bean names (such as an OAuth-enabled web client) with the corresponding YAML configuration associated with C2C onboarding.
5. Payload Structures & Partner Specifications
• Separate payload models: Amazon and Arsenal require separate profile-specific payload implementations based on their respective models and API specifications, including the co-brand partner notifications API.
• Future scalability: The modular, profile-based approach should also support future partner integrations, such as Marriott.
6. Closing & Next Steps
• Continue investigating the codebase offline.
• Review the application’s message-consumption and Kafka-publishing paths, and identify generic versus partner-specific components.
• Consider reconnecting the following day to clarify open questions.
Action Items
Item
Follow-up
Dev testing
Assess the profile-based dev controller approach for looping requests back into the application for logging and confirmation.
Configuration
Verify partner-specific profiles, YAML properties, OAuth-enabled web client bean names, and required authorization-token setup.
Architecture
Map integration points and classify code as generic, Amazon-specific, or Arsenal-specific.
Refactoring
Identify shared publisher logic and define interfaces/abstract classes for partner-specific implementations.
Payloads
Confirm Amazon and Arsenal payload models against their respective API specifications.
Follow-up discussion
Reconnect the next day if further clarification is needed.
Kong Proxy Onboarding & Architecture Alignment  •  Page
