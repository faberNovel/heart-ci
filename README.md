# About Heart

Heart is an open-source website quality analysis tool developed by _Fabernovel_, relying on a variety of third-party services such as _Mozilla Observatory_ or _Google Lighthouse_.

For more details, see [Heart's official website](https://heart.fabernovel.com) and [Heart's official GitLab repository](https://gitlab.com/fabernovel/heart).

# How to use Heart CI workflows ?

1) In your web project, define a configuration folder, and within it a `dareboost` folder. The default configuration directory path is `<root>/ci/heart/`.

2) In the `dareboost` folder, define one JSON configuration per analysis to be run. Refer to [Dareboost's official documentation](https://www.dareboost.com/en/documentation-api) to see the list of available parameters and overall syntax of the configuration file. For more clarity, JSON configuration files can be organized in subfolders. This is a simple example:

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

3) In the settings of your GitHub repository, define the following secrets :

```
DAREBOOST_API_TOKEN=<your-dareboost-api-token>

SLACK_API_TOKEN=<your-slack-api-token>

SLACK_CHANNEL_ID=<name-of-the-slack-channel-to-notify>
```

# Example workflow call

```
# To be updated
```