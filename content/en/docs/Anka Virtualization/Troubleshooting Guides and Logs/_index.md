---
title: "Troubleshooting Guides and Logs"
linkTitle: "Troubleshooting Guides and Logs"
weight: 1
hideSubPagesInLeftMenu: false
---

> Anka Virtualization + CLI < 2.0 are EOL and no longer supported

We recommend using our diagnostic package collection script and then provide the output archive to support. This script collects logs and usage statistics from the host. It's recommended that this is run at the time a problem is happening. Run the following command on the machine experiencing issues:

```bash
curl https://raw.githubusercontent.com/veertuinc/scripts/main/generate-anka-node-diagnostics.bash -O && \
chmod +x generate-anka-node-diagnostics.bash && \
./generate-anka-node-diagnostics.bash
```

