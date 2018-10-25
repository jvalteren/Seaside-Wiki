Release Early. Release Often

 1. Update the version number of Grease/Seaside in the code
  * GRPlatform >> #version
  * GRPlatform >> #seasideVersion
 1. Test the release
   1. Run the unit tests and make sure they all pass. (This is done using Travis-CI)
   1. Click through the functional test suite using the all server adaptors and all different encodings.
 1. Update the Metacello config [TODO: needs updating]
  * see `ConfigurationOfSeaside3 >> #DevelopmentProcess`
  * see `ConfigurationOfGrease >> #DevelopmentProcess`
 1. Load `Package-Dependencies` and `GraphViz` from `http://source.lukas-renggli.ch/packaging`. Generate the graph `WADevelopment graphFunctionalDependencies: true tests: true` and make sure the dependencies are valid. [TODO: still valid?]
 1. Prepare a list of [[resolved issues|Seaside305Changelog]].
 1. Upload image/changes in a ZIP and one-click distribution to website
  * locally under `/srv/seaside.st/web/distributions`
  * update links
   * `http://www.seaside.st/download/pharo`
   * `http://www.seaside.st/design/parts/download`
 1. Announce the release in the mailing-list and on twitter.
 1. Update [[Past Releases|Past-Releases]]. 
 1. Update and fix this list.
 1. Repeat forever.