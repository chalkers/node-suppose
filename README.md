Node.js - suppose
=================

Have you ever heard of the command line program [expect][1]? Basically, `expect` allows you to automate command line programs. `suppose` is a programmable Node.js module that allows the same behavior.



Why?
----

From the [expect wikipedia][1] page, you can see many examples of `expect` scripts automating tasks such as `telnet` or `ftp` solutions. Now you can easily write Node.js scripts to do the same. This may be most beneficial during testing.



Installation
------------

    npm install suppose



Example
------

Automate the command `npm init`, which initializes a new npm module.

```javascript
process.chdir('/tmp/awesome');
suppose('npm', ['init'])
  .on('name: (awesome) ').respond('awesome_package\n')
  .on('version: (0.0.0) ').respond('0.0.1\n')
  .on('description: ').respond("It's an awesome package man!\n")
  .on('entry point: (index.js) ').respond("\n")
  .on('test command: ').respond('npm test\n')
  .on('git repository: ').respond("\n")
  .on('keywords: ').respond('awesome, cool\n')
  .on('author: ').respond('JP Richardson\n')
  .on('license: (BSD) ').respond('MIT\n')
  .on('ok? (yes) ' ).respond('yes\n')
.end(function(code){
    assert(code === 0);
    var packageFile = '/tmp/awesome/package.json';
    fs.readFile(packageFile, function(err, data){
        var packageObj = JSON.parse(data.toString());
        assert(packageObj.name === 'awesome_package');
        assert(packageObj.version === '0.0.1');
        assert(packageObj.description === "It's an awesome package man!");
        assert(packageObj.main === 'index.js');
        assert(packageObj.scripts.test === 'npm test');
        assert(packageObj.keywords[0] === 'awesome');
        assert(packageObj.keywords[1] === 'cool');
        assert(packageObj.author === 'JP Richardson');
        assert(packageObj.license === 'MIT');
        done();
    });
});
```

Always follow an `.on()` with a `.respond()` and then finish with a `.end()`.


License
-------

(MIT License)

Copyright 2012, JP Richardson



[1]: http://en.wikipedia.org/wiki/Expect