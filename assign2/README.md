## Assignment 2 - Create Continuous Integration Environment

### Prerequsites
- Student has Github account
- Student has Node JS installed on local laptop

### Concepts Introduced
- Continuous Build
- Continuous Integration
- Working with AWS CLI
- Test Driven Development (TDD)

### Step 1 - Fork Sample Project into Student Github Repo
`
$ git clone https://github.com/hackoregon/programmingforprogress-frontend.git
$ cd programmingforprogress-frontend
$ git fork
`
### Step 2 - Setup Travis/GitHub Integration
A. Log in to http://travis-ci.org using your github credentials
B. ![Add Repositories](/images/1.png)
C. ![Activate Repository](/images/2.png)

### Step 3 - Travis Installation

`sudo gem install travis`

### Step 4 - AWS Configuration
A. ![Create Access Keys](/images/3.png)
B. ![Create Access Keys](/images/4.png)
C. ![Create Bucket](/images/5.png)

### Step 5 - Configure Travis build config file
Create .travis.yml file and save to project directory
`
language: node_js
node_js:
    - "6.0"
install: npm install

deploy:
  provider: s3
  access_key_id: "<AWS Key ID>"
  secret_access_key: "<AWS Key ID>"
  bucket: "<Your BucketName>"
`
### Step 5 - Push changes & check build
`
$ git add .
$ git commit . -m "added travis configuration"
`
Switch to travis page and watch build. It will fail.

### Step 6 - Install Mocha & Setup Tests


change travis config to install mocha framework
`
language: node_js
node_js:
    - "6.0"
before_install: npm install mocha
install: npm install

deploy:
  provider: s3
  access_key_id: "<AWS Key ID>"
  secret_access_key: "<AWS Key ID>"
  bucket: "<Your BucketName>"
`
From your local command line:
`
$ npm install mocha
`
Edit package.json file in project directory to this:
`
"scripts": {
  "test":"mocha",
`
create a stubbed unit test function
`
$ mkdir test
`
in your favorite editor create the file test/test.js with the following:
`
var assert = require('assert');
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      assert.equal(-1, [1,2,3].indexOf(4));
    });
  });
});
`
from your command line run:

`$ npm test`
You should see:
`
Array
  #indexOf()
    ✓ should return -1 when the value is not present

1 passing (12ms)
`
Commit your changes
`
$ git add .
$ git commit . -m "added travis configuration"
`
Switch to travis page and watch build. It should pass.
