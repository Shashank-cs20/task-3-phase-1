1. Dev writes code, commits to feature branch
2. Opens PR
3. Someone reviews manually
4. Dev merges to main (manually)
5. Someone SSHs into server, pulls latest code (manually)
6. Restarts the app manually

 #Define the gates (build, test) each change must pass
Gate 1: Build   → does it compile/build without errors?
Gate 2: Test    → do unit/integration tests pass?
Gate 3: Lint    → does it meet code style rules? (optional but common)

on: [pull_request]
jobs:
  build-and-test:
    steps:
      - run: docker build .
      - run: npm test
If any gate fails, the PR cannot merge (tie this back to the branch protection rules you set up earlier).

#Ensure each deploy is traceable to a commit
docker build -t my-app:$(git rev-parse --short HEAD).

#Plan a rollback path
Ask: If this deploy breaks something, how do we go back — and how fast?
docker run my-app:<previous-commit-sha>

git revert the bad commit and let CI/CD redeploy automatically.

1. Identify last known-good commit SHA
2. Run: docker run my-app:<that-sha>
3. Confirm service is healthy

#Verification check:

Push a commit that fails a test → confirm the pipeline blocks it and doesn't deploy.
Look at a running container → can you tell exactly which commit it's running, in under 10 seconds?
Simulate a rollback → time how long it takes you to revert to the previous version.
Hand the lifecycle doc to a teammate and ask them to explain it back to you in their own words.


<img width="1536" height="1024" alt="screenshot 2112" src="https://github.com/user-attachments/assets/3c4cd31e-f3ff-4c0d-a736-9b916786b539" />

