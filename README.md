# About Heart

Heart is an open-source website quality analysis tool developed by _Fabernovel_, relying on a variety of third-party services such as _Mozilla Observatory_ or _Google Lighthouse_.

For more details, see [Heart's official website](https://heart.fabernovel.com) and [Heart's official GitLab repository](https://gitlab.com/fabernovel/heart). 

# List of provided workflows

## standard-batch-analysis.yaml (reusable)

Intended for use with the **fabernovel/heart:standard** docker image, which supports Heart's Dareboost and Slack modules. This workflow performs the following jobs:
- Loop over all JSON files included in the defined configuration directory (and its subdirectories).
- Pull the specified Docker image from DockerHub and cache it.
- For each JSON configuration file, launch a Heart analysis through a Docker container. A successful analysis will result in a notification on the indicated Slack channel. Unsuccessful analyses will be indicated in Github Actions' run logs.

## example.yaml

Simple example of workflow making use of the above workflow.

# How to use Heart CI workflows on your project ?

1) In your web project, define a configuration folder, and within it a `dareboost` folder. The default configuration directory path is `ci/heart/config/`.

2) In the `dareboost` folder, define one JSON configuration per analysis to be run. Refer to [Dareboost's official documentation](https://www.dareboost.com/en/documentation-api) to see the list of available parameters and overall syntax of the configuration file. For more clarity, JSON configuration files can be organized in subfolders. Here is a simple example of configuration:

```yaml
{
    "url": "https://heart.fabernovel.com/",
    "lang": "fr",
    "location": "Paris",
    "browser": {
        "name": "iPhone 6s/7/8 (BETA)"
    },
    "mobileAnalysis": true,
    "isPrivate": true
}
```

3) In the settings of your GitHub repository, define the secrets to be used inside your workflows.

```bash
DAREBOOST_API_TOKEN=<your-dareboost-api-token>

SLACK_API_TOKEN=<your-slack-api-token>

# Ensure that this Slack channel allows notifications by Heart
SLACK_CHANNEL_ID=<name-of-the-slack-channel-to-notify>
```

4) Navigate to your Github workflow repository and manually download the workflows that you need:

```bash

# Standard batch analysis reusable workflow
curl -s -O https://raw.githubusercontent.com/faberNovel/heart-ci/main/.github/workflows/standard-batch-analysis.yaml

# Example workflow making use of the above
curl -s -O https://raw.githubusercontent.com/faberNovel/heart-ci/main/.github/workflows/example.yaml

```



# Example using heart-ci workflows

```yaml
name: Website performance analysis

# Github Events Documentation: 
on:
  # Example 1
  push
    branches:
      - '**'
      # - 'preprod'

  # Example 2
  workflow_dispatch: ~
  schedule:
    # Every Monday at 10:00 AM
    - cron: '0 10 * * 1'

jobs:

  # Run a series of Heart analyses based on configuration files
  analysis:
    uses: ./.github/workflows/standard-batch-analysis.yaml
    with:
      heart-image-name: fabernovel/heart:standard
    secrets:
      DAREBOOST_API_TOKEN: your-secret-dareboost-token
      SLACK_API_TOKEN: your-secret-slack-api-token
      SLACK_CHANNEL_ID: the-secret-slack-channel-to-notify
```