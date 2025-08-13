AI IDE Rules for Building DIY AlertR Alarm Monitoring System
1️⃣ General Principles
Follow the Project Specs 100%

Never create features, interfaces, or integrations that were not explicitly agreed upon.

Stick to what’s in the roadmap, checklist, and user instructions.

Ask Questions if Unclear

If the AI encounters ambiguity (e.g., a sensor type, config value, or UI detail), pause and request clarification before proceeding.

Stay Modular & Incremental

Always break tasks into small, testable units.

Complete one module fully before moving to the next.

Do Not “Daydream” or Make Assumptions

Avoid creating hypothetical code, features, or workflows outside what’s documented.

Only implement what is explicitly approved.

Continuous Cross-Checking

If stuck, review:

Project roadmap

Architecture diagram

Checklist of steps

Existing codebase or configs

Do not continue blindly.

2️⃣ Coding Rules
Follow Standards

Use consistent style, naming conventions, and comments.

Python: PEP8 compliant

JavaScript: consistent formatting (camelCase for variables, PascalCase for components)

Error Handling

Never leave unhandled exceptions.

Always log errors with enough info to debug.

Security First

All credentials and API keys must be stored securely (environment variables).

Always use HTTPS/TLS for external communications.

No Hardcoding Values

Use config files or environment variables for:

Ports

DB credentials

API keys

Sensor mappings

Testing

Each module must have test cases or simulation before integrating:

Edge Bridge → fake sensor events

Push Server → test JSON payloads

Dashboard → mock API responses

3️⃣ Communication & Integration
Always Verify Connections

Before moving to the next step, confirm:

Edge Bridge successfully sends to AlertR

AlertR logs event in DB

Push Server sends notification correctly

Dashboard displays live events

Event Mapping Must be Accurate

SIA codes, JSON fields, and sensor IDs must match agreed mapping.

No Silent Failures

Every failed connection, parsing error, or invalid event must be logged and reported.

API Contracts

Always stick to defined JSON payloads and endpoints.

Never change field names, structure, or endpoints without approval.

4️⃣ UI Rules
Only Build Approved Screens

Dashboard / Live Feed

Event Detail

Reports / Analytics

Sensor/User Management

Consistent Color Coding

Red = active

Orange = warning

Yellow = acknowledged

Green = resolved

Interactive Workflow

Buttons must behave as defined: Acknowledge, Assign, Resolve, Escalate.

Operator actions always update AlertR server and database in real-time.

No Extra Animations or UI Components

Only implement agreed components and layouts.

5️⃣ Workflow Rules
Follow the Phase-by-Phase Roadmap

Do not skip phases (Environment → AlertR → Edge Bridge → Push → UI → Integration → Security → Optional Enhancements)

Test Before Proceeding

Each phase must be verified before moving to the next.

If test fails, troubleshoot and log before continuing.

Document Everything

Code comments for logic

Config explanations

Instructions for deployment, testing, and integration

Backup Regularly

Save configs, DB schemas, and scripts after every major change.

6️⃣ Problem-Solving Rules
If Stuck

Step back, review all documents: roadmap, architecture, checklist.

Check previous modules and logs.

Ask clarifying questions to human operator if needed.

Do Not Guess

Never create new logic to “fill in gaps” unless approved.

Iterate Carefully

Only make changes that align with the defined workflow.

Always maintain compatibility with previous modules.

7️⃣ Safety & Reliability
Secure All Sensitive Data

No credentials in code repository

No exposing AlertR endpoints to the internet without HTTPS and auth

Logging

Every module logs actions, errors, and event flow.

Logs must be readable and timestamped.

Fail-Safe Behavior

If a module crashes, it must not corrupt the DB or lose events.

Push Server must queue events until delivery succeeds.

8️⃣ Human Collaboration
Always defer to the human operator for approval of:

New features

Data format changes

UI redesigns

Provide clear reports of:

Completed modules

Test results

Remaining tasks