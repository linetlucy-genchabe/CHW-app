# CHT Commodity Request 

A Community Health Toolkit (CHT) app for managing commodity requests with CHW submission and CHA approval workflows.

## Features

- **CHWs** submit requests for essential medical commodities.
- **CHAs** review and approve/reject CHW requests.
- Hierarchical contact structure:
  - `County → Subcounty → Community Unit → CHA → CHW`

---

## Setup Guide

### 1. Prerequisites

- CHT instance (v4.x or later)
- Docker & Docker Compose (for local dev)
- Git & GitHub account
- `curl -s -o cht-docker-compose.sh https://raw.githubusercontent.com/medic/cht-core/master/scripts/docker-helper-4.x/cht-docker-compose.sh`
- `./cht-docker-compose.sh` (for deploying forms/config)

---

### 2. Clone Project

```bash
git clone https://github.com/linetlucy-genchabe/CHW-app.git
cd CHW-app
```
### 3. Project Structure 
```bash
configuration/
├── forms/
│   ├── contact/
│   │   ├── contact-form.xlsx
│   │   ├── contact-form.xml
│   │   └── contact-form.json
│   └── commodity-request/
│       ├── commodity-request.xlsx
│       ├── commodity-request.xml
│       └── commodity-request.json
├── resources/
│   └── app-settings.json
.gitignore
README.md
```
### 4. Deploy to CHT
```bash
./cht-docker-compose.sh config/.docker-env
```

### 5. Form Specifications
###### 1. Commodity Request Form (forms/commodity-request.xml)
Used by: CHWs

Fields:

- Commodity type (dropdown: MRDTs, AL6s, etc.)

- Quantity (1-99)

- Auto-generated:

- Submission ID (CHWName-CommodityType-Date)

- Request date

###### 2. CHA Approval Form (forms/cha_approval.xml)
Used by: CHAs

Fields:

- Request ID (auto-linked)

- Approval status (Approve/Reject)

- Comments

### 6. Workflow
CHW Submission
- CHW logs in → Sees "Request Commodities" task.

- Completes commodity-request form.

- Submission triggers a task for supervising CHA.

CHA Approval
- CHA logs in → Sees "Approve Requests" task.

- Reviews request and fills cha_approval form.

- System updates status, notifies CHW.

### 7. Troubleshooting
| Issue               | Solution                                                 |
| ------------------- | -------------------------------------------------------- |
| CHA can't see tasks | Ensure CHA contact is linked to a valid community unit   |
| Forms not appearing | Check form IDs in `app-settings.json` and form filenames |
| Invalid hierarchy   | Verify UUIDs and parent-child links in contact form      |
| Sync delays         | Run `./cht-docker-compose.sh restart` or reset the browser session    |

