# JBrowse BroadcastChannel plugin

Plugin to integrate [BroadcastChannel](https://developer.mozilla.org/en-US/docs/Web/API/BroadcastChannel) functionality with JBrowse.

### Links

[GitHub repository](https://github.com/legumeinfo/jbrowse-plugin-broadcastchannel/)

[Running example](http://dev.lis.ncgr.org:50071/jbrowse2/)
&bull; [GCV instance to talk to it](http://dev.lis.ncgr.org:50071/gcv2/gene;lis=arahy.Tifrunner.gnm1.ann1.R8SYM2?q=arahy.Tifrunner.gnm1.ann1.R8SYM2&sources=lis&algorithm=repeat&match=10&mismatch=-1&gap=-1&score=30&threshold=25&bmatched=20&bintermediate=10&bmask=10&linkage=average&cthreshold=20&neighbors=10&matched=4&intermediate=5&bregexp=&border=chromosome&regexp=&order=distance)
<br/>(both on dev-zzbrowse, make sure they use the same channel)

### Notes

Reverse proxy is in `/etc/httpd/conf.d/jbrowse.conf` on dev-zzbrowse (50071).

### To do

Does it handle select/deselect (which GCV expects) correctly?

How to make the plugin persist? (it just does?)

Dot plot: waiting for GCV to be able to send two species/strains

Set initial state from configuration file

Click on features, not just mouseover?

Fix unit tests (so GitHub does not post a red X).

List BroadcastChannel proxy before JBrowse2 proxy in `proxy-conf/jbrowse.conf` (?)
