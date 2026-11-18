# Assignment 1 — Deploy a Storage Account via Bicep (on floci-az)

## 🎯 Goal
Write a Bicep template and deploy a **Storage Account** to a **Resource Group**
using the `az` CLI — all against the **local floci-az emulator** (not real Azure).

---

## 📂 Files
```
floci-azure/
├── main.bicep   ← Your Bicep template 
└── README.md    ← This file
```

---

## 🚀 Step-by-step Commands

### Step 1 — Wire up the emulator

> Run this every time you open a new terminal.
> It sets the env vars that point `az` at the local emulator instead of real Azure.

```bash
eval "$(floci az env -o text)"
```

Then verify you're pointed at the emulator (should NOT say `AzureCloud`):

```bash
az cloud show --query name
```

---

### Step 2 — Create a Resource Group

```bash
az group create --name my-rg --location westeurope
```

Verify it exists:

```bash
az group list -o table
```

---

### Step 3 — Validate your Bicep before deploying

Catch syntax errors before they hit the emulator:

```bash
az bicep build --file main.bicep
```

Then do a preflight validation against the emulator:

```bash
az deployment group validate \
  --resource-group my-rg \
  --template-file main.bicep
```

---

### Step 4 — Deploy

```bash
az deployment group create \
  --resource-group my-rg \
  --template-file main.bicep \
  --name assignment1-deploy
```

---

### Step 5 — Verify the Storage Account exists

```bash
az storage account list --resource-group my-rg -o table
```

---

### Step 6 — Check deployment details

```bash
az deployment group show \
  --resource-group my-rg \
  --name assignment1-deploy \
  -o table
```

---

## ✅ You're done when
- [ ] `az group list` shows `my-rg`
- [ ] `az storage account list` shows your storage account
- [ ] `az deployment group show` shows `Succeeded`

---

## 🧹 Cleanup (after you're satisfied)
```bash
az group delete --name my-rg --yes
```

---

