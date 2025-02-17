## Tool YAML

The Tool YAML file defines the configuration and behavior of your tool. Here is a template you need to edit:

```yaml
tools:
  # 1. Change this to the name of your tool   
  - name: list_s3_buckets
    image: python:3.11
    # 2. Update the description to fit your tool's purpose
    description: "List all S3 buckets in your AWS account using the specified AWS CLI profile."
    # 3. Change this alias to a short, easy-to-type name for your tool
    alias: list-s3-buckets
    content: |
      # 4. Update the URL to point to your repository
      REPO_URL="${REPO_URL:-https://github.com/your-repo/your-project}"

      # 5. Change this to the name of your repository
      REPO_NAME="${REPO_NAME:-your-project}"

      # 6. Specify the directory where your source code is located
      SOURCE_CODE_DIR="${SOURCE_CODE_DIR:-src/tool-example}"

      # 7. Set this to the branch you want to use (e.g., main, dev)
      REPO_BRANCH="${REPO_BRANCH:-main}"

      # 8. Directory name for cloning the repo
      REPO_DIR="${REPO_DIR:-$REPO_NAME}"

      BIN_DIR="${BIN_DIR:-/usr/local/bin}"
      APT_CACHE_DIR="${APT_CACHE_DIR:-/var/cache/apt/archives}"
      PIP_CACHE_DIR="${PIP_CACHE_DIR:-/var/cache/pip}"

      # Create cache directories (DO NOT REMOVE THE CREATION OF CACHE DIRECTORIES)
      mkdir -p "$APT_CACHE_DIR"
      mkdir -p "$BIN_DIR"
      mkdir -p "$PIP_CACHE_DIR"

      # 9. Add more dependencies here if needed
      install_git() {
        apt-get update -qq > /dev/null && apt-get install -y -qq git > /dev/null
      }

      install_pip_dependencies() {
        export PIP_CACHE_DIR="$PIP_CACHE_DIR"
        pip install boto3 --cache-dir "$PIP_CACHE_DIR" --quiet > /dev/null
      }

      # Install git
      install_git

      # Install pip dependencies
      install_pip_dependencies

      # Clone repository if not already cloned
      if [ ! -d "$REPO_DIR" ]; then
        if [ -n "$GH_TOKEN" ]; then
          GIT_ASKPASS_ENV=$(mktemp)
          chmod +x "$GIT_ASKPASS_ENV"
          echo -e "#!/bin/sh\nexec echo \$GH_TOKEN" > "$GIT_ASKPASS_ENV"
          GIT_ASKPASS="$GIT_ASKPASS_ENV" git clone --branch "$REPO_BRANCH" "https://$GH_TOKEN@$(echo $REPO_URL | sed 's|https://||')" "$REPO_DIR" > /dev/null
          rm "$GIT_ASKPASS_ENV"
        else
          git clone --branch "$REPO_BRANCH" "$REPO_URL" "$REPO_DIR" > /dev/null
        fi
      fi

      # cd into the cloned repo
      cd "${REPO_DIR}/${SOURCE_CODE_DIR}"

      # Run the script
      export PYTHONPATH="${PYTHONPATH}:/${REPO_DIR}/${SOURCE_CODE_DIR}"

      # 10. Update this to match the script's location and name within your repository
      exec python list_s3_buckets.py --profile "{{ .profile }}"
    args:
      # 11. Define the command-line arguments for your script
      - name: profile
        # 12. Update the description to explain what the argument is
        description: 'AWS CLI profile name'
        required: true
    env:
      # 13. List environment variables required by your tool
      - AWS_PROFILE
    # 14. Only include this section if your tool requires specific files, such as AWS credentials (which are passed to the teammate if you add the AWS_PROFILE environment variable)
    with_files:
      - source: $HOME/.aws/credentials
        destination: /root/.aws/credentials
```

### Customization Steps

#### 1. **Name**
   - **Action:** Change the name of the tool to something unique and descriptive.

#### 2. **Description**
   - **Action:** Update the description to explain what the tool does.

#### 3. **Alias**
   - **Action:** Change the alias to a short, memorable name for the tool.

---

### Within Content

#### 4. **Repo URL**
   - **Action:** Set the URL of your repository.

#### 5. **Repo Name**
   - **Action:** Change to the name of your repository.

#### 6. **Source Code Directory**
   - **Action:** Specify the directory containing your source code.

#### 7. **Repo Branch**
   - **Action:** Set to the branch you want to use (e.g., main, dev).

#### 8. **Repo Directory**
   - **Action:** Directory name for cloning the repository.

#### 9. **Dependencies**
   - **Action:** Add any additional dependencies needed in the `install_git` function.

#### 10. **Exec Python**
   - **Action:** Update the command to run your specific script within the cloned repository.

#### 11. **Args**
   - **Action:** Define the command-line arguments for your script, including names and descriptions.

#### 12. **Env**
   - **Action:** List any environment variables your tool requires AWS_PROFILE can be passed to the teammate if you have setup the AWS integration within the Kubiya web app.

---

### With Files

> **Note:** Only include this section if your tool needs to access specific files, like AWS credentials (which are passed to the teammate if you add the AWS_PROFILE environment variable)
