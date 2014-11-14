# Development and Release

Usually, mobile software development can be separated into two steps development and release. In development stage, developer should be able to push apk directly to device by either IDE( intellij, Eclipse or Android studio). It's not necessary to build whole android ROM during this stage.

When release, it may be necessary to submit the apk to Onyx for system integration as the apk may require root privilege. Onyx uses github for source code review and integration. So developer should be able to fork the repository and submit the pull request (https://help.github.com/articles/using-pull-requests/) to Onyx team. Once merged, the 3rd party developer should be able to build android ROM for integration test.

# Build Android ROM

Onyx provides android builder server, and developer should be able to access the build server from 
http://oa.o-in.me:9015/ and http://oa.o-in.me:9016/. If you don't have the account, please submit your request to Onyx support. 




