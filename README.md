### Hi there 👋
![Static Badge](https://img.shields.io/badge/Ohad_Taizi--bleak)

![npm](https://img.shields.io/npm/v/react)
![Static Badge](https://img.shields.io/badge/React--green?logo=react&logoColor=61DBFB&labelColor=black)
![Static Badge](https://img.shields.io/badge/JavaScript--yellow?logo=JavaScript&logoColor=yellow&labelColor=black)
![Static Badge](https://img.shields.io/badge/Java--red?logo=Java&logoColor=red&labelColor=black)
![Static Badge](https://img.shields.io/badge/C%2B%2B--pink?logo=C%2B%2B&logoColor=pink&labelColor=black)
![Static Badge](https://img.shields.io/badge/C--gold?logo=C&logoColor=gold&labelColor=black)

import requests
from collections import defaultdict

# Replace 'your_github_username' and 'your_github_token' with your GitHub username and personal access token
username = "ohadtaizi"
token = "your_github_token"

# Function to get all repositories including forks
def get_repos(username, token):
    repos_url = f"https://api.github.com/users/{username}/repos?per_page=100"
    headers = {"Authorization": f"token {token}"}
    repos = []
    while repos_url:
        response = requests.get(repos_url, headers=headers)
        repos.extend(response.json())
        repos_url = response.links.get('next', {}).get('url')
    return repos

# Function to get language statistics for a repository
def get_repo_languages(repo, token):
    languages_url = repo["languages_url"]
    headers = {"Authorization": f"token {token}"}
    response = requests.get(languages_url, headers=headers)
    return response.json()

# Fetch all repositories
repos = get_repos(username, token)

# Calculate total language usage
languages = defaultdict(int)
for repo in repos:
    repo_languages = get_repo_languages(repo, token)
    for lang, bytes_count in repo_languages.items():
        languages[lang] += bytes_count

# Calculate total bytes across all languages
total_bytes = sum(languages.values())

# Print language usage percentages
for lang, bytes_count in languages.items():
    print(f"{lang}: {bytes_count / total_bytes * 100:.2f}%")

# Optionally, save the result to a README file or another format
with open("language_usage.md", "w") as f:
    f.write("# Language Usage Statistics\n")
    for lang, bytes_count in languages.items():
        f.write(f"{lang}: {bytes_count / total_bytes * 100:.2f}%\n")

<!--
**ohadtaizi/ohadtaizi** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
