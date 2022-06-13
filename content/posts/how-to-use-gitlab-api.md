+++
title = "How to use the GitLab API"
summary = "An introduction to GitLab's API for when you have to handle too many repositories at once"
date = 2022-01-07T19:45:00-03:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["Guides"]
tags = ['gitlab', 'api', 'code']
featuredImage = "https://about.gitlab.com/images/press/logo/png/gitlab-logo-gray-rgb.png"
+++

# How to Use the GitLab API

Sometimes, when you don't use a monorepo, it may be necessary to handle multiple repositories at once.
GitLab provides an easy way to do that, even with a free account!
All you have to do is find what you need in their [documentation](https://docs.gitlab.com/ee/api/).

The first step, is to create an access token, for that go to the [profile](https://gitlab.com/-/profile) page
 and find the [Access Tokens' settings](https://gitlab.com/-/profile/personal_access_tokens).
There you'll be able to create a new token, with a configurable permission scope and expiration date.

Lets say, for example, that you want to clone every repository from the group **ID: 3253154**.

![GitLab Group ID: 3253154](/tales-tips-and-tricks/images/gitlab-group-id.png)

To do that you can use the ["List a group's projects"](https://docs.gitlab.com/ee/api/groups.html#list-a-groups-projects),
which can be achieved with a `read_api` permission. 

```sh
curl --request GET --header "PRIVATE-TOKEN: this-should-be-your-token" "https://gitlab.com/api/v4/groups/$PROJECT_ID/projects?simple=true" > /tmp/projects.json
```

This will save a JSON with the ID, URL, name, and path of each project.
You can use Python to handle it and print the SSH urls to clone, like so:

```python
import json
with open('/tmp/projects.json') as fp:
    group_info = json.load(fp)

for project_info in group_info:
    print("git clone", project_info['ssh_url_to_repo'])
```

You can also use `subprocess.run()` to clone them.

If you want to, you can even make requests using Python directly,
for example, to update every projects' Merge Request settings to use merge commits and forbid squash: 

```python
import requests
token = 'this-should-be-your-token'
for project_info in group_info:
    requests.put(
        f'https://gitlab.com/api/v4/projects/{project_info['id']}',
        headers={'PRIVATE-TOKEN':token},
        data={'merge_method':'merge', 'squash_option': 'never'},
    )
```

That's it. There's plenty more in their documentation, as I've said before, be creative and let your problems guide you.
