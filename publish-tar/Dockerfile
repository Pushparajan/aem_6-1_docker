# DOCKER-VERSION 1.7.0 PUBLISH
FROM aem_6-2_base
LABEL version="1.0"
LABEL description="AEM publish docker image"
MAINTAINER pramar

#Copies required build media
ADD resources/*.jar /aem/publish/cq-publish-4502.jar
ADD resources/license.properties /aem/publish/license.properties
ADD resources/postInstallHook.py /aem/publish/postInstallHook.py
ADD resources/packagelist.txt /aem/publish/packagelist.txt
COPY resources/packages/ /aem/publish/packages
#RUN ls -l /aem/publish/packages

# Extracts AEM
WORKDIR /aem/publish
RUN java -Djava.awt.headless=true -XX:MaxPermSize=256m -Xmx1280m -jar cq-publish-4502.jar -unpack -r nosamplecontent

# Add customised log file, to print the logging to standard out.
#ADD resources/org.apache.sling.commons.log.LogManager.config /aem/publish/crx-quickstart/install/org.apache.sling.commons.log.LogManager.config
#ADD resources/org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService.cfg /aem/publish/crx-quickstart/install/org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService.cfg
COPY resources/osgi/ /aem/publish/crx-quickstart/install/osgi

# Installs AEM
WORKDIR /aem/publish
RUN ["python","/aem/aemInstaller.py","-i","cq-publish-4502.jar","-r","publish","-p","4502"]

WORKDIR /aem/publish/crx-quickstart/bin
RUN mv quickstart quickstart.original
ADD resources/quickstart /aem/publish/crx-quickstart/bin/quickstart
RUN chmod +x /aem/publish/crx-quickstart/bin/quickstart
RUN chmod +x /aem/publish/compaction.sh

EXPOSE 4502
ENTRYPOINT ["/aem/publish/crx-quickstart/bin/quickstart"]
#ENTRYPOINT ["/aem/publish/compaction.sh"]
