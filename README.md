# webb23-alm-todo-app


# Step 1
- Create .env file and add two env variables
    ```code
    MONGODB_URI=
    TEST_MONGODB_URI=
    ```
- Try running the test locally on you computer and make sure it connect to you mongodb test db that is in you .env
    ```bash
    npm test
    ```

# Step 2
- Lets create the github action worklow
    - create the folder .github and the workflows inside it
    ```
        .github/
            workflows/
    ```
    - Inside the workflows is where will create our first action yaml file
    ```        
        .github/
            workflows/
                test-action.yml
    ```
    - test-action.yml code
    ```yaml
    name: Test Workflow
    on:
    push:
        branches:
        - main
    pull_request:
        branches:
        - main
        # Defines the events that trigger the workflow. 
        # This workflow will run on pushes to the main branch 
        # and pull requests targeting the main branch.
    jobs:
    build:
        runs-on: ubuntu-latest
        # Specifies the type of machine to run the job on. 
        # Here, it's running on the latest version of Ubuntu.
        environment: test
        # set the enviroment 
        steps:
        - name: Checkout repository
        uses: actions/checkout@v2
        # Checks out your repository code.

        - name: Install Node.js
        uses: actions/setup-node@v2
        with:
            node-version: 'lts/*'
        # Sets up Node.js environment with the latest LTS version.

        - name: Install dependencies
        run: npm install
        # Installs your project dependencies using npm.

        - name: Create .env file
        run: |
            cat << EOF >> ./.env
            TEST_MONGODB_URI=${{ secrets.TEST_MONGODB_URI }}
            EOF
            # This command uses the cat utility along with a here document 
            # (<< EOF) to append text to a file named .env. 
            # The << EOF syntax indicates the start of the here document,
            # and EOF indicates the end. Anything between << EOF and 
            # EOF will be appended to the .env file.

        - name: Run tests
        run: npm test
        # Executes your test script.
    ```
