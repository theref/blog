# theref.eth Blog

A simple Hugo-based blog deployed to IPFS, accessible at [theref.eth](https://theref.eth.link).

Read about the setup in [A Censorship Resistant Blog](https://theref.eth.link/posts/blog-setup/).

## Setup

This uses the [Archie theme](https://github.com/athul/archie) as a git submodule.

```bash
# Clone with submodules
git clone --recursive https://github.com/theref/blog.git

# Or if already cloned, initialize submodules
git submodule update --init --recursive
```

## Local Development

```bash
# Install Hugo (if not already installed)
brew install hugo

# Run local server
hugo server -D

# Build site
hugo --minify
```

The site will be available at `http://localhost:1313`

## Deployment

Automatically deploys to IPFS on every push to `main` via GitHub Actions.

The workflow:
1. Builds the Hugo site
2. Deploys the `public/` directory to IPFS using Storacha
3. Pins to Pinata for persistent availability
4. Posts the IPFS hash as a comment on PRs

**Note:** ENS `contenthash` updates are currently manual. After a deployment, I take the new CID and update the ENS record for `theref.eth`.

### Required Secrets

Set these in GitHub Settings → Secrets and Variables → Actions → New repository secret:

#### Setting up Storacha secrets

1. Install the Storacha CLI:
   ```bash
   npm install -g @storacha/cli
   ```

2. Create a key for CI (outputs JSON):
   ```bash
   w3 key create --json
   ```
   Copy the `key` field value → set as `STORACHA_KEY` secret

3. Login and create a space:
   ```bash
   w3 login your-email@example.com
   w3 space create blog
   ```

4. Create a delegation proof (replace `DID_OF_KEY` with the DID from step 2):
   ```bash
   w3 delegation create did:key:DID_OF_KEY \
     -c space/blob/add \
     -c space/index/add \
     -c filecoin/offer \
     -c upload/add \
     --base64
   ```
   Copy the output → set as `STORACHA_PROOF` secret

**Important**: Both secrets must be base64-encoded strings, not file paths!

#### Setting up Pinata (for persistent pinning)

1. Create a free account at [pinata.cloud](https://pinata.cloud)
2. Go to API Keys → New Key
3. Give it a name (e.g., "GitHub Actions") with Admin privileges
4. Copy the JWT token → set as `PINATA_JWT_TOKEN` secret
