# Blog

A simple Hugo-based blog deployed to IPFS.

## Setup

This uses the [Archie theme](https://github.com/athul/archie) as a git submodule.

```bash
# Clone with submodules
git clone --recursive <repo-url>

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
4. Updates IPNS name with w3name (static address for ENS)
5. Posts the IPFS hash as a comment on PRs

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

#### Setting up w3name (IPNS for static ENS address)

This creates a permanent IPNS name that you can point your ENS to once, and it will auto-update on each deploy.

1. **Generate a signing key**:
   ```bash
   # Create a temp directory for key generation
   mkdir -p /tmp/w3name-keygen && cd /tmp/w3name-keygen
   npm init -y
   npm install w3name
   node --input-type=module -e "import * as Name from 'w3name'; const n = await Name.create(); console.log('W3NAME_NAME=', n.toString()); console.log('W3NAME_KEY_HEX=', Buffer.from(n.key.raw).toString('hex'));"
   cd - && rm -rf /tmp/w3name-keygen
   ```
   
2. Copy the `W3NAME_KEY_HEX` value → set as `W3NAME_SIGNING_KEY` secret

3. **Get your IPNS name** (after first deploy):
   - Check the GitHub Action output for your IPNS name (starts with `k51...`)
   - Point your ENS content hash to this IPNS name
   - Your ENS will now automatically resolve to the latest content!

## Structure

```
content/       # Your blog posts and pages
themes/        # Hugo themes (Archie)
public/        # Built site (generated, not committed)
hugo.toml      # Hugo configuration
```

## Writing Posts

Create new posts in `content/posts/`:

```bash
hugo new posts/my-post.md
```

Edit the post, then commit and push to deploy.
