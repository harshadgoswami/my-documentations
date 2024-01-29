Syncing a forked repository involves updating your fork with changes made to the original (upstream) repository. Here are the general steps to sync a forked repository:

### Using Git Command Line:

1. **Clone Your Fork:**

   ```bash
   git clone https://github.com/your-username/your-forked-repo.git
   cd your-forked-repo
   ```

2. **Add Upstream Remote:**

   ```bash
   git remote add upstream https://github.com/original-owner/original-repo.git
   ```

3. **Fetch Upstream Changes:**

   ```bash
   git fetch upstream
   ```

4. **Merge Upstream Changes:**

   ```bash
   git merge upstream/main
   ```

   Replace `main` with the branch you want to sync.

5. **Push Changes to Your Fork:**
   ```bash
   git push origin main
   ```
   Replace `main` with the branch you are syncing.

### Using GitHub Desktop:

1. **Open GitHub Desktop:**
   Open GitHub Desktop and select your forked repository.

2. **Repository > Repository Settings:**
   Go to "Repository" in the top menu and choose "Repository Settings."

3. **Add Upstream Repository:**
   Click on "Add" under the "Remote" section, and add the URL of the original repository as the upstream.

4. **Fetch Upstream Changes:**
   Click on "Repository" and choose "Fetch origin" to get changes from the upstream repository.

5. **Merge Upstream Changes:**
   In the "Changes" tab, select "current branch" and click on "Update from main" (replace `main` with the branch you are syncing).

6. **Push Changes to Your Fork:**
   Click on "Repository" and choose "Push origin" to push the changes to your fork.

### Using GitHub Web Interface:

1. **Open a Pull Request from Upstream to Your Fork:**

   - Navigate to the original repository (upstream).
   - Click on the "Pull Requests" tab.
   - Click on the "New Pull Request" button.
   - Set the base repository to your fork and the base branch to the branch you want to sync.
   - Create the pull request but don't merge it.

2. **Merge the Pull Request Locally:**

   - Go to your forked repository on GitHub.
   - Navigate to the "Pull Requests" tab.
   - Find the open pull request from upstream and click on it.
   - Click on the "Merge" button to merge the changes into your fork.

3. **Update Local Repository:**
   - If you are working locally, fetch the changes and merge them into your local branch.
   - Push the changes to your fork on GitHub.

These steps should help you keep your forked repository in sync with the original repository. Adjust branch names as needed.
