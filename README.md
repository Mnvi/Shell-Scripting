# Shell-Scripting
Shell
# Github API Integration

#!/bin/bash

# GitHub API URL
GITHUB_API_URL="https://api.github.com"

# GitHub Personal Access Token (replace with your token or use environment variables)
GITHUB_TOKEN="your_github_token_here"

# Function to fetch user information
fetch_user_info() {
    local username=$1
    if [ -z "$username" ]; then
        echo "Usage: fetch_user_info <username>"
        return 1
    fi

    curl -s -H "Authorization: token $GITHUB_TOKEN" \
         "$GITHUB_API_URL/users/$username" | jq .
}

# Function to list user repositories
list_user_repos() {
    local username=$1
    if [ -z "$username" ]; then
        echo "Usage: list_user_repos <username>"
        return 1
    fi

    curl -s -H "Authorization: token $GITHUB_TOKEN" \
         "$GITHUB_API_URL/users/$username/repos" | jq .
}

# Main script logic
if [ "$#" -lt 2 ]; then
    echo "Usage: $0 <command> <username>"
    echo "Commands:"
    echo "  fetch_user_info - Fetch user information"
    echo "  list_user_repos - List user repositories"
    exit 1
fi

COMMAND=$1
USERNAME=$2

case $COMMAND in
    fetch_user_info)
        fetch_user_info "$USERNAME"
        ;;
    list_user_repos)
        list_user_repos "$USERNAME"
        ;;
    *)
        echo "Unknown command: $COMMAND"
        exit 1
        ;;
esac
