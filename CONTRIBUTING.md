## Contributing
### Contribution Rules
1. No SNAPSHOT dependencies on "develop" and obviously "master" branches

### Releasing
1. From the develop branch, prepare a release (example using a HTTP proxy):
<pre><code>
    $ mvn -Dhttps.proxyHost=proxyhostname -Dhttps.proxyPort=8080 jgitflow:release-start
</code></pre>
1. Update the CHANGELOG according to keepachangelog.com.
1. To perform the release (example using a HTTP proxy):
<pre><code>
    $ mvn -Dhttps.proxyHost=proxyhostname -Dhttps.proxyPort=8080 jgitflow:release-finish
</code></pre>
    If, after deployment, the command does not succeed because of some issue with the branches. Fix the issue, then re-run the same command but with 'noDeploy' option set to true to avoid re-deployment:
<pre><code>
    $ mvn -Dhttps.proxyHost=proxyhostname -Dhttps.proxyPort=8080 -DnoDeploy=true jgitflow:release-finish
</code></pre>
1. Connect and log in to the OSS Nexus Repository Manager: https://oss.sonatype.org/
1. Go to Staging Profiles and select the pending repository authzforce-*... you just uploaded with `jgitflow:release-finish`
1. Click the Release button to release to Maven Central.
1. Build and publish the Docker image to Docker Hub (`version` is the last release version):
   ```shell
   $ git checkout release-${version} 
   $ cd  cxf-spring-boot-server
   $ docker build -t authzforce/restful-pdp:${version} .
   $ docker login
   $ docker push authzforce/restful-pdp:${version}
   ```

More info on jgitflow: http://jgitflow.bitbucket.org/
