DIY AlertR Alarm System: Complete Beginner Checklist
Step 0 — Prep Your Machine
Install Python 3.9+

bash
Copy code
sudo apt update
sudo apt install python3 python3-pip python3-venv git
Install a database (PostgreSQL recommended)

bash
Copy code
sudo apt install postgresql postgresql-contrib
Create a database and user:

sql
Copy code
CREATE DATABASE alertr_db;
CREATE USER alertr_user WITH PASSWORD 'strongpassword';
GRANT ALL PRIVILEGES ON DATABASE alertr_db TO alertr_user;
Optional: create a virtual environment

bash
Copy code
python3 -m venv alertr_env
source alertr_env/bin/activate
Step 1 — Install AlertR Server
Clone repo:

bash
Copy code
git clone https://github.com/sqall01/alertR.git
cd alertR
Install dependencies:

bash
Copy code
pip install -r requirements.txt
Configure server.conf (or .env) with:

text
Copy code
DB_HOST=localhost
DB_NAME=alertr_db
DB_USER=alertr_user
DB_PASS=strongpassword
LISTEN_PORT=5000
API_KEY=supersecretkey
Start server:

bash
Copy code
python alertr_server.py
Test status:

bash
Copy code
curl http://localhost:5000/status
# Should return "running"
Step 2 — Set Up Edge Bridge (Translator + SIA)
UDP (SIA) Example
Create edge_bridge_udp.py (code snippet from earlier)

Configure port in code:

python
Copy code
AJAX_UDP_PORT = 10000
ALERTR_SERVER_URL = "http://localhost:5000/events"
Run:

bash
Copy code
python edge_bridge_udp.py
Test with fake SIA message:

bash
Copy code
echo "110 001 202508141230" | nc -u -w1 localhost 10000
HTTP (Translator API) Example
Create edge_bridge_http.py (Flask code snippet from earlier)

Run:

bash
Copy code
python edge_bridge_http.py
Test with curl:

bash
Copy code
curl -X POST http://localhost:5005/ajax-webhook \
-H "Content-Type: application/json" \
-d '{"device":"motion_001","event":"motion","timestamp":"2025-08-14T12:30:00"}'
Step 3 — Push Server
Create push_server.py (code snippet from earlier)

Configure email / FCM / SMS credentials

Run:

bash
Copy code
python push_server.py
Test with JSON payload:

bash
Copy code
curl -X POST http://localhost:5001/push-event \
-H "Content-Type: application/json" \
-d '{
  "sensor_id":"motion_001",
  "event_type":"motion_triggered",
  "timestamp":"2025-08-14T12:30:00",
  "emails":["operator@example.com"],
  "mobile_tokens":["fcm_device_token"]
}'
Verify email / push notifications arrive.

Step 4 — Database Logging
Tables (PostgreSQL example):

sql
Copy code
CREATE TABLE Sensors (
  sensor_id VARCHAR PRIMARY KEY,
  location VARCHAR,
  type VARCHAR,
  zone VARCHAR
);

CREATE TABLE Events (
  event_id SERIAL PRIMARY KEY,
  sensor_id VARCHAR REFERENCES Sensors(sensor_id),
  event_type VARCHAR,
  timestamp TIMESTAMP,
  status VARCHAR
);

CREATE TABLE Notifications (
  notif_id SERIAL PRIMARY KEY,
  event_id INT REFERENCES Events(event_id),
  sent_to VARCHAR,
  channel VARCHAR,
  status VARCHAR
);

CREATE TABLE Users (
  user_id SERIAL PRIMARY KEY,
  username VARCHAR,
  role VARCHAR,
  contact_info VARCHAR
);
Test: insert a fake event and query:

sql
Copy code
INSERT INTO Events(sensor_id, event_type, timestamp, status)
VALUES('motion_001','motion_triggered','2025-08-14 12:30:00','active');

SELECT * FROM Events;
Step 5 — Dashboard / UI
Choose framework:

React.js (modern, interactive)

Flask + Jinja2 (simple, Python stack)

Main pages:

Live Dashboard: list alarms, color-coded by severity.

Event Detail: sensor info, timeline, notes, action buttons.

Reports / Analytics: charts, filters, export to PDF/CSV.

Sensor / User Management: add/edit sensors, assign zones, manage users.

Connect to AlertR server API to fetch live events and status.

Step 6 — Integrate Ajax System
SIA UDP

Configure Ajax hub → send SIA to Edge Bridge IP + port

Example SIA message:

Copy code
110 001 202508141230
Translator API

Configure Ajax hub → POST JSON to /ajax-webhook

Example JSON:

json
Copy code
{
  "device": "motion_001",
  "event": "motion",
  "timestamp": "2025-08-14T12:30:00"
}
Confirm events appear in AlertR server → push server → dashboard.

Step 7 — Operator Workflow
New alarm appears on dashboard (red card / map pin)

Click Acknowledge → turns yellow

Assign responder if needed

Resolve → green, logs stored

Optionally auto-generate incident report emailed to client

Step 8 — Security & Hardening
Use TLS / HTTPS for all APIs

Strong passwords and API keys

Role-based access: Operator / Admin / Viewer

Audit logging: who acknowledged/resolved events and when

Backup database regularly

Step 9 — Optional Enhancements
Multi-site support

Mobile apps for operators

Camera integration for AI verification

SMS gateway integration

Dashboard heatmaps for alarm density

This checklist gives you a clear path from empty machine → fully working system, with all configs, example JSON, and commands included.