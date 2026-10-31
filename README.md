# Mega Project

College mega-project workspace.

## ZeroTrustAI — API Security Platform

The working API security prototype is in
[`ZeroTrustAI_API_Security_Platform/`](ZeroTrustAI_API_Security_Platform/).

```text
Mega-Project/
├── README.md
└── ZeroTrustAI_API_Security_Platform/
    ├── gateway/
    ├── demo_api/
    ├── dashboard/
    ├── ml/
    ├── tests/
    ├── compose.yaml
    └── README.md
```

### Run locally

```bash
git clone https://github.com/Shreyashmhatre/Mega-Project.git
cd Mega-Project/ZeroTrustAI_API_Security_Platform
python3 scripts/init_env.py
docker compose up -d --build
```

Open http://127.0.0.1:8000. Full setup, testing, and known limitations are documented
in the [project README](ZeroTrustAI_API_Security_Platform/README.md).
