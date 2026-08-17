# Host a did:web DID Document on an Azure Static Web App

## 1. Prepare a GitHub Repository

1. Create (or reuse) a GitHub repo, e.g. `Formula5DID`.
2. At the **root** of the repo, create a folder named:

   ```
   .well-known
   ```

3. Inside `.well-known`, create a file named:

   ```
   did.json
   ```

4. Paste your DID document into `did.json` and commit to your main branch.

---

## 2. Validate the DID Document

1. Ensure the JSON is valid (no trailing commas, proper quotes).
2. Confirm the DID matches your domain, for example:

   ```json
   {
     "id": "did:web:yourdomain.com",
     "verificationMethod": [
       {
         "id": "did:web:yourdomain.com#key-1",
         "type": "JsonWebKey2020",
         "controller": "did:web:yourdomain.com",
         "publicKeyJwk": { "...": "..." }
       }
     ],
     "authentication": [
       "did:web:yourdomain.com#key-1"
     ],
     "assertionMethod": [
       "did:web:yourdomain.com#key-1"
     ]
   }
   ```

3. Commit any fixes.

---

## 3. Create the Azure Static Web App

1. In the Azure portal, select **Create a resource → Static Web App**.
2. Choose subscription and resource group.
3. Name the app (e.g., `formula5-did`).
4. Select **GitHub** as the deployment source.
5. Choose your repo and branch.
6. Build configuration:
   - **App location:** `/`
   - **Api location:** *(leave empty)*
   - **Output location:** *(leave empty)*

7. Create the Static Web App and wait for deployment.

---

## 4. Verify the DID Document on the Default URL

1. Open the Static Web App URL, e.g.:

   ```
   https://<app-name>.azurestaticapps.net
   ```

2. Navigate to:

   ```
   https://<app-name>.azurestaticapps.net/.well-known/did.json
   ```

3. Confirm the JSON DID document is returned (not HTML, not 404).

---

## 5. Configure a Custom Domain for did:web

> `did:web` requires the DID document to be served from the domain encoded in the DID.

1. Choose your DID domain, e.g.:
   - `yourdomain.com` → `did:web:yourdomain.com`
   - `did.identity.yourdomain.com` → `did:web:did.identity.yourdomain.com`

2. In the Static Web App:
   - Go to **Custom domains → Add**.
   - Add a **CNAME** record in your DNS pointing to the Static Web App hostname.

3. Wait for DNS validation.
4. Enable **HTTPS** for the custom domain.

---

## 6. Verify the did:web Endpoint

1. Open:

   ```
   https://yourdomain.com/.well-known/did.json
   ```

   or

   ```
   https://did.identity.yourdomain.com/.well-known/did.json
   ```

2. Confirm:
   - The JSON loads correctly.
   - The `id` field matches your DID.

---

## 7. Use the DID in Microsoft Entra Verified ID

1. In **Entra admin center → Verified ID**:
   - Set your Issuer/Verifier DID to:

     ```
     did:web:yourdomain.com
     ```

2. Save the configuration.
3. Test a Verified ID flow to confirm the DID resolves.

---
