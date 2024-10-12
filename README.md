Simple example of setting up CI/CD for a Node.js project using GitHub Actions for CI and deploying to Vercel for CD.

### Step 1: Set Up Your Node.js Project

1. **Create a New Node.js Project**:
   ```bash
   mkdir my-hobby-project
   cd my-hobby-project
   npm init -y
   npm install express jest --save
   ```

2. **Create a Simple Express App**:
   Create a file named `app.js`:
   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/', (req, res) => {
       res.send('Hello, World!');
   });

   app.listen(PORT, () => {
       console.log(`Server is running on port ${PORT}`);
   });

   module.exports = app; // Export the app for testing
   ```

3. **Add a Simple Test**:
   Create a folder named `__tests__` and add a file named `app.test.js`:
   ```javascript
   const request = require('supertest');
   const app = require('../app');

   test('GET / should respond with Hello, World!', async () => {
       const response = await request(app).get('/');
       expect(response.text).toBe('Hello, World!');
       expect(response.statusCode).toBe(200);
   });
   ```

4. **Update `package.json` for Testing**:
   Add a test script to `package.json`:
   ```json
   "scripts": {
       "test": "jest"
   }
   ```

### Step 2: Set Up GitHub Actions for CI

1. **Create a `.github/workflows/ci.yml` File**:
   Create the directory and file:
   ```bash
   mkdir -p .github/workflows
   touch .github/workflows/ci.yml
   ```

   Add the following code to `ci.yml`:
   ```yaml
   name: CI

   on:
     push:
       branches:
         - main
     pull_request:
       branches:
         - main

   jobs:
     test:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout code
           uses: actions/checkout@v2

         - name: Set up Node.js
           uses: actions/setup-node@v2
           with:
             node-version: '14'

         - name: Install dependencies
           run: npm install

         - name: Run tests
           run: npm test
   ```

### Step 3: Set Up Deployment with Vercel

1. **Create a Vercel Account**: Sign up at [Vercel](https://vercel.com).

2. **Install Vercel CLI** (if you want to deploy via command line):
   ```bash
   npm install -g vercel
   ```

3. **Deploy Your Project**:
   Run the following command and follow the prompts:
   ```bash
   vercel
   ```

4. **Set Up Automatic Deployments**:
   - In your Vercel project settings, link your GitHub repository.
   - Vercel will automatically deploy your project every time you push to the main branch.

### Final Steps

1. **Commit Your Code**:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin <your-repo-url>
   git push -u origin main
   ```

2. **Observe CI/CD in Action**:
   - Push changes to your repository, and GitHub Actions will run your tests.
   - If all tests pass, Vercel will automatically deploy your application.

### Conclusion

With these steps, you've set up a basic CI/CD pipeline for a hobby project using GitHub Actions for continuous integration and Vercel for deployment. As you get comfortable, you can expand this setup with more advanced features, like environment variables, more complex workflows, or additional testing strategies!