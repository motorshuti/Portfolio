Steps from creating a new GitHub repository to setting up a CI/CD pipeline with GitHub Actions, including a security check using Snyk:
*Step 1: Create a new GitHub repository*
1.	Sign in to GitHub: Go to GitHub and sign in to your account.(motorshuti)
2.	Create a new repository:
•	Click on the "+" icon in the top right corner of the page.
•	Select "New repository" from the dropdown menu.
3.	Fill in the repository details:
•	Enter a name for your repository (here, Portfolio).
•	Add a description if you like.
•	Choose whether the repository will be public or private.
•	Initialize this repository with a README (optional).
•	Click on the "Create repository" button.
*Step 2: Clone your repository to your local machine*
bash
Copy code
git clone https://github.com/motorshuti/Portfolio.git cd Portfolio 
*Step 3: Create a Jekyll site*
Next, initialize a new Jekyll site:
bash
Copy code
gem install bundler jekyll jekyll new . 
*Step 4: Set up your Jekyll site*
Modify your Gemfile to include the necessary gems:
ruby
Copy code
gem 'github-pages', group: :jekyll_plugins 
*Step 5: Create a GitHub Actions workflow*
Create a new file named .github/workflows/final.yml with the following content:
yaml
name: Deploy to GitHub Pages with Security Check

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Setup Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: 3.3.1

    - name: Install dependencies
      run: |
        gem install bundler
        bundle install

    - name: Build Jekyll site
      run: bundle exec jekyll build

    - name: Run security check
      uses: snyk/actions/node@master
      with:
        args: test
      env:
        SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

    - name: Deploy to GitHub Pages
      id: deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.MAD}}
        publish_dir: ./_site

    - name: Show URL
      run: echo "Website deployed at https://motorshuti.github.io/Portfolio/"

    - name: Set output URL
      run: echo "::set-output name=url::https://motorshuti.github.io/Portfolio/"

    - name: Output URL
      run: echo "Deployed to ${{ steps.deploy.outputs.url }}"

*Step 6: Get your Snyk token*
1.	Sign in to Snyk: Go to Snyk and sign in to your account.
2.	Generate your API token:
•	Click on your profile icon in the top right corner.
•	Select "Account settings" from the dropdown menu.
•	In the left sidebar, click on "API token".
•	Click on the "Generate API token" button.
•	Give your token a name (e.g., "GitHub Actions") and click "Generate".
•	Copy the generated token.
*Step 7: Add your Snyk token as a secret to your GitHub repository*
1.	Go to your GitHub repository.
2.	Go to "Settings" > "Secrets".
3.	Click on "New repository secret".
4.	Name your secret SNYK_TOKEN.
5.	Paste your Snyk API token value.
6.	Click "Add secret".
*Step 8: Commit and push your changes*
