## Assignment 2 - Create Continuous Integration Environment

### Prerequsites
- Student has Github account
- Student has Node JS installed on local laptop

### Concepts Introduced
- Continuous Build
- Continuous Integration
- Working with AWS CLI
- Test Driven Development (TDD)

### Step 1 - Fork sample project
```bash
$ git clone https://github.com/hackoregon/programmingforprogress-frontend.git
$ cd programmingforprogress-frontend
$ git fork
```
### Step 2 - Setup Travis/GitHub integration
1. Log in to http://travis-ci.org using your github credentials
2. Add your repository
3. Activate the repository **<github user name>/programmingforprogress-frontend**

### Step 3 - Travis Installation
```bash 
sudo gem install travis
```
### Step 4 - AWS Configuration
1. [Do Steps 1-3 in AWS tutorial to setup bucket policy](http://docs.aws.amazon.com/gettingstarted/latest/swh/setting-up.html)
2. [Follow the instructions to create your AWS CLI credentials](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html)

### Step 5 - Configure Travis build config file
Create .travis.yml file and save to project directory
```yaml
language: node_js
node_js:
    - "6.0"
install: npm install

deploy:
  provider: s3
  access_key_id: "<AWS Key ID>"
  secret_access_key: "<AWS Key ID>"
  bucket: "<Your BucketName>"
```
### Step 5 - Push changes & check build
```bash
$ git add .
$ git commit . -m "added travis configuration"
```
Switch to travis page and watch build. It will fail.

### Step 6 - Install Mocha & Setup Tests

Change travis config to install mocha framework
```yaml
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
```

From your local command line:
```bash
$ npm install mocha
```

Edit package.json file in project directory to this:
```javascript
"scripts": {
  "test":"mocha",
```

create a stubbed unit test function

```bash
$ mkdir test
```

in your favorite editor create the file test/test.js with the following:

```javascript
var assert = require('assert');
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      assert.equal(-1, [1,2,3].indexOf(4));
    });
  });
});
```

from your command line run:

```bash
$ npm test
```

You should see:
```bash
Array
  #indexOf()
    ✓ should return -1 when the value is not present

1 passing (12ms)
```

Commit your changes from the command line

```bash
$ git add .
$ git commit . -m "added travis configuration"
```

Switch to travis page and watch build. It should pass.
