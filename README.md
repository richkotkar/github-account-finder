# GitHub Account Finder

A small client-side web app to look up GitHub user profiles by username and display profile information plus links to their most recent 5 repositories.

Description
-----------
When searching GitHub users in "GitHub Account Finder," you can enter a GitHub username to retrieve the user's public profile information (avatar, name, bio, location, followers, following, public repo count, etc.) and quick links to their most recent 5 repositories.

Built with: HTML, CSS, JavaScript

Table of contents
-----------------
- [Features](#features)
- [Demo](#demo)
- [Getting started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Run locally](#run-locally)
- [Usage](#usage)
- [How it works (internals)](#how-it-works-internals)
- [Configuration / GitHub API notes](#configuration--github-api-notes)
- [Project structure](#project-structure)
- [Development & contribution guidelines](#development--contribution-guidelines)
- [Accessibility & testing](#accessibility--testing)
- [Security](#security)
- [License](#license)
- [Acknowledgements](#acknowledgements)
- [Contact](#contact)

Features
--------
- Enter a GitHub username and fetch profile details.
- Show avatar, display name, bio, location, followers, following, public repo count.
- List the user's latest 5 repositories (name + short description + link).
- Responsive, lightweight UI built with plain HTML/CSS/JavaScript.
- Graceful error handling (user not found, rate limits, network errors).

Demo
----
(Replace the screenshot/GIF below with a real screenshot or hosted demo link if available.)

![screenshot-placeholder](./assets/screenshot.png)

Getting started
---------------
These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

Prerequisites
- A modern browser (Chrome, Firefox, Edge, Safari).
- (Optional) Node.js and npm if you want to use a small dev server workflow or add build steps.
- No backend required — this is a client-side app that uses the public GitHub REST API.

Installation
------------
1. Clone the repo
   git clone https://github.com/richkotkar/github-account-finder.git
2. Open the project folder
   cd github-account-finder

Run locally
-----------
Option A — Static (quick):
- Open `index.html` in your browser. Note: some browsers block fetch on file:// for security; use a local server for consistent behavior.

Option B — Simple HTTP server (recommended):
- Python 3:
  python -m http.server 8000
  then open http://localhost:8000 in your browser.
- Node (http-server):
  npm install -g http-server
  http-server -c-1
  open http://localhost:8080

Option C — VS Code Live Server extension:
- Install Live Server and choose "Open with Live Server" on `index.html`.

Usage
-----
1. Enter a GitHub username in the search box (example: `torvalds`, `octocat`).
2. Press Search (or Enter).
3. The app displays the user's profile card and a list of their 5 most recently updated public repositories. Click a repo to open it on GitHub.

How it works (internals)
------------------------
At a high level the app performs two requests to the GitHub REST API:

1. GET user profile:
   https://api.github.com/users/{username}

2. GET user repos (sorted by most recently updated, limited to 5):
   https://api.github.com/users/{username}/repos?per_page=5&sort=updated

A minimal fetch example used by the app:

```js
async function fetchUser(username, token) {
  const headers = token ? { Authorization: `token ${token}` } : {};
  const profileRes = await fetch(`https://api.github.com/users/${username}`, { headers });
  if (!profileRes.ok) throw new Error(`User fetch failed: ${profileRes.status}`);
  const profile = await profileRes.json();

  const reposRes = await fetch(`https://api.github.com/users/${username}/repos?per_page=5&sort=updated`, { headers });
  if (!reposRes.ok) throw new Error(`Repos fetch failed: ${reposRes.status}`);
  const repos = await reposRes.json();

  return { profile, repos };
}
