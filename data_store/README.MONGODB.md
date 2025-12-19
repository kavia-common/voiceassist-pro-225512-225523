# MongoDB Data Store

This container initializes MongoDB and standardizes environment variables for downstream services.

Standard env vars:
- MONGODB_URL: Full MongoDB connection URL (e.g., mongodb://user:pass@localhost:5000/?authSource=admin)
- MONGODB_DB: Target database name (e.g., myapp)

Startup:
- Run data_store/startup.sh (respects MONGODB_URL and MONGODB_DB if provided; otherwise uses defaults)
- Connection helper is written to data_store/db_connection.txt (contains: "mongosh <connection-string>")

Database initialization:
- Collections created: users, calls, recordings, prompts, configs, audit_logs
- Indexes:
  - users: email unique
  - calls: created_at, status
  - audit_logs: created_at
- Seed documents: one minimal document inserted into each collection

DB Viewer:
- Environment file written to data_store/db_visualizer/mongodb.env with MONGODB_URL and MONGODB_DB
- Start the simple viewer: 
  source data_store/db_visualizer/mongodb.env && (cd data_store/db_visualizer && npm start)

Notes:
- Use db_connection.txt to run additional mongosh commands:
  $(cat data_store/db_connection.txt) --eval "db.collection.findOne({})"
