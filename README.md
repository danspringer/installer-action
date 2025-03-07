# installer-action
An action for GitHub to upload your REDAXO AddOn automatically into the REDAXO installer with every GitHub release based on your GitHub repository and main branch.

# Usage

## Preconditions

> **Please note:** The AddOn must already be created in MyREDAXO. 

* A valid MyREDAXO user. Create a new login: https://redaxo.org/registrierung/ 
* A valid API key from https://redaxo.org/myredaxo/mein-api-key/
* Your AddOn created in MyAddons https://redaxo.org/myredaxo/meine-addons/

> **Notice** You don't need to be part of FriendsOfREDAXO to use this action. Just follow the instructions for evers repository.

## 1: Adding Secrets

> This step is not required for REDAXO AddOns within the FriendsOfREDAXO GitHub organization. Continue with step 2. 

Add your MyREDAXO credentials to your organization secrets or repository secrets, e.g. via `https://github.com/YOUR_GITHUB_NAME/YOUR_REPOSITORY/settings/secrets/actions` ([GitHub Docs](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository)).

| secret name | value |
| ----------- | ----------- |
| `MYREDAXO_USERNAME` | Your REDAXO login user |
| `MYREDAXO_API_KEY` | Your MyREDAXO API key |

![Secrets](https://user-images.githubusercontent.com/16903055/170964550-9ac8c80f-cd9f-4ead-956d-1c63e24df8ac.png)


example value for `MYREDAXO_API_KEY` named secret:

![API-Key](https://user-images.githubusercontent.com/16903055/170967258-f53082cd-e827-4a86-bed6-1233f34dd4b9.png)

## 2: Create a new GitHub action workflow 

Create a new release workflow by creating a new file in your repository called `.github/workflows/publish-to-redaxo-org.yml` with the following content or follow the instructions on https://github.com/YOUR_GITHUB_NAME/YOUR_REPOSITORY/actions/new.

![image](https://user-images.githubusercontent.com/3855487/158783007-d2e11062-d4ef-4ada-ae23-8c2a8228e4c5.png)


### .github/workflows/publish-to-redaxo-org.yml


```yaml
# Instructions: https://github.com/FriendsOfREDAXO/installer-action/

name: Publish to REDAXO.org
on:
  release:
    types:
      - published

jobs:
  redaxo_publish:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: FriendsOfREDAXO/installer-action@v1
      with:
        myredaxo-username: ${{ secrets.MYREDAXO_USERNAME }}
        myredaxo-api-key: ${{ secrets.MYREDAXO_API_KEY }}
        description: ${{ github.event.release.body }}
        version: ${{ github.event.release.tag_name }}
        
```

## 3. Create a new release with a nice description 

As soon as you publish a new release on GitHub, this workflow is triggered and your AddOn is uploaded to myREDAXO.

We recommend to create a new tag with the same name of the version defined in the `package.yml` of your REDAXO AddOn.

We also recommend to use a nice description with information about the date of the relase, new features or instructions for other developers, e.g. any important changes or tasks to do before or after the installation. It's possible to use markdown syntax.

```markdown 
# My Addon Name | Published on XX.XX.202X | Version 

One line about the main new feature.

> **Important:** An warning about breaking changes, new preconditions or additional instructions

## What's new

* A list of features or changes with reference to contributing GitHub users , e.g. @friendsofredaxo 
* A list of features or changes with reference to contributing GitHub users , e.g. @friendsofredaxo 
* A list of features or changes with reference to contributing GitHub users , e.g. @friendsofredaxo 

## Changes

* A list of features or changes with reference to contributing GitHub users , e.g. @friendsofredaxo 
* A list of features or changes with reference to contributing GitHub users , e.g. @friendsofredaxo 

Changelog: https://www.example.org/
```

## 4. Watch and enjoy the smooth action workflow 

You can monitor the status of the workflow under https://github.com/YOUR_GITHUB_NAME/YOUR_REPOSITORY/actions. Once completed, the result is your REDAXO AddOn published through MyREDAXO and live in ever REDAXO 5 installer.

![image](https://user-images.githubusercontent.com/3855487/161302304-176fa2d0-5101-45cb-aabe-328aef2f10c7.png)
