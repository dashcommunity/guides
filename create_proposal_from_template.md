# Create a proposal using a template

### Create a proposal repository
1. Sign in to Github and go to [dash community main page](https://github.com/dashcommunity)
2. Click **New repository**
3. Fill out the form:
    * *Owner*: `dashcommunity`
    * *Repository name*: `proposal-<name-you-will-give-proposal-on-blockchain>`, e.g. `proposal-core-team` (omit `<` and `>`)
      * Keep consistent with the naming restrictions of blockchain (char. limit: 20, valid char.: A-Z,a-z,0-9,-)
    * *Description*: `three to ten word summary of project`, e.g. `Salary for core development team` 
    * *Initialize this repository with a README* box: `checked`
4. Click **Create repository** button

### Scaffold your repository
1. Click **Create new file**
2. Enter `template.md` in the *name your file* field (don't forget the `.md`)
3. Click **Commit new file**
4. Click **Create new file**
5. Enter `proposal.md` in the *name your file* field
6. Click **Commit new file**

### Populate your repository
1. Go to the [templates repository](https://github.com/dashcommunity/proposal-templates)
2. Select a template, e.g. [`detailed.md`](https://github.com/dashcommunity/proposal-templates/blob/master/detailed.md)
3. Click **raw**
4. Copy the entire text of the template
5. Return to your project repository, e.g. [proposal-dash-community](https://github.com/dashcommunity/proposal-dash-community)
6. Click **`template.md`** file you created in the last step
7. Click the **pencil icon** ("Edit this file" on hover)
8. Paste in the contents of your clipboard
9. Click **Commit change**
10. Repeat steps 5 through 9 for the `proposal.md` file (give it the same contents)
  * `template.md` remains unedited in your repo for reference
  * `proposal.md` starts out the same as `template.md`, but then you edit it
    
### Edit your `proposal.md`
1. Return to your project repository, e.g. [proposal-dash-community](https://github.com/dashcommunity/proposal-dash-community)
2. Click **`proposal.md`** file you created in the last step
3. Read the templete to get an idea of what goes where in the proposal
4. Click the **pencil icon** ("Edit this file" on hover)
5. Delete the **Usage & Content** fiels entirely (you can refer back to your `template.md` if needed)
6. Delete the **Description** sections entirely
7. Delete the **Example** *headings*, leave the section *content* for now
8. Select all the text and copy
9. Click **Commit changes**

### Initialize a [Github Project Page](https://help.github.com/articles/creating-pages-with-the-automatic-generator/)
1. Return to your project repository, e.g. [proposal-dash-community](https://github.com/dashcommunity/proposal-dash-community)
2. Click **Settings**
3. Scroll down to the *GitHub Pages* module
4. Click **Launch automatic page generator**
5. Replace the content of the automatcally-generated *body* field with the content of your clipboard
  * The content of your clipboard is the sample template you copied in the previous step 
  * Keep the default *Project name* and *Tagline* text
6. Play around with editing the populated document, experimenting with the markdown menu features
7. Click **Continue to layouts**
8. Choose a theme ([proposal-dash-community](https://github.com/dashcommunity/proposal-dash-community) uses **Midnight** to illustrate a dark theme)
9. Review, make changes if desired by clicking **edit**, and click **Publish** when finished
10.  Note the message above ("Your project page has been created at...")
11. Open a browser and go to the URL Github made for you

### Re-edit `proposal.md` and re-launch Github Project Page as needed
1. Return to your project repository, e.g. [proposal-dash-community](https://github.com/dashcommunity/proposal-dash-community)
2. Click **`proposal.md`**, then the **edit icon**
3. Modify the text of each section for your proposed project
4. Select all text and copy
5. Click the green **Commit changes** button
5. Repeat the steps in [Initialize a Github Project Page](link)
