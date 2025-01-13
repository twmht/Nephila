## Add local git hook
To add the Git hook functionality (e.g., preventing direct pushes to the `main` branch) using `pre-commit`, you can create a custom hook and integrate it into your `.pre-commit-config.yaml` file.

### Steps to Add the Hook Using `pre-commit`

1. **Install `pre-commit`** (if not already installed):
    
    ```bash
    pip install pre-commit
    ```
    
2. **Initialize `pre-commit` in Your Repository**:
    
    ```bash
    pre-commit install
    ```
    
3. **Create a Custom Hook Script**: Save the following script as `.git-hooks/pre-push` in your repository:
    
    ```bash
    #!/bin/bash
    while read local_ref local_sha remote_ref remote_sha
    do
        if [[ "$remote_ref" == "refs/heads/main" ]]; then
            echo "Direct pushes to the main branch are not allowed."
            exit 1
        fi
    done
    ```
    
    Make the script executable:
    
    ```bash
    chmod +x .git-hooks/pre-push
    ```
    
4. **Add the Hook to `.pre-commit-config.yaml`**: Update or create the `.pre-commit-config.yaml` file with the following configuration:
    
    ```yaml
    repos:
      - repo: local
        hooks:
          - id: prevent-push-to-main
            name: Prevent Push to Main Branch
            entry: .git-hooks/pre-push
            language: system
            stages: [push]
    ```
    
5. **Install the Hook**: Run the following command to install the hook:
    
    ```bash
    pre-commit install --hook-type pre-push
    ```
    
6. **Test the Hook**: Try to push directly to the `main` branch. The hook should prevent the push and display an error message.
    

---

### Explanation of Configuration

- **`repo: local`**: Indicates that this is a custom hook stored locally in your repository.
- **`hooks`**: Defines the list of hooks to use.
    - **`id`**: A unique identifier for the hook.
    - **`name`**: A descriptive name for the hook.
    - **`entry`**: Path to the script to execute.
    - **`language: system`**: Specifies that the script is a shell script.
    - **`stages: [push]`**: Ensures this hook only runs during the `pre-push` stage.

This approach integrates the hook management into `pre-commit`, ensuring it is consistently applied across different development environments.