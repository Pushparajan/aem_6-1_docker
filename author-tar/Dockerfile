# DOCKER-VERSION 1.7.0 AUTHOR
FROM aem_6-2_base
LABEL version="1.0"
LABEL description="AEM author docker image"
MAINTAINER pramar

#Copies required build media
ADD resources/*.jar /aem/author/cq-author-4502.jar
ADD resources/license.properties /aem/author/license.properties
ADD resources/postInstallHook.py /aem/author/postInstallHook.py
ADD resources/packagelist.txt /aem/author/packagelist.txt
COPY resources/packages/ /aem/author/packages
RUN ls -l /aem/author/packages

# Extracts AEM
WORKDIR /aem/author
RUN java -Djava.awt.headless=true -XX:MaxPermSize=256m -Xmx1280m -jar cq-author-4502.jar -unpack -r nosamplecontent

# Add customised log file, to print the logging to standard out.
#ADD resources/org.apache.sling.commons.log.LogManager.config /aem/author/crx-quickstart/install/org.apache.sling.commons.log.LogManager.config
#ADD resources/org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService.cfg /aem/author/crx-quickstart/install/org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService.cfg
COPY resources/osgi/ /aem/author/crx-quickstart/install/osgi

# Installs AEM
WORKDIR /aem/author
RUN ["python","/aem/aemInstaller.py","-i","cq-author-4502.jar","-r","author","-p","4502"]

WORKDIR /aem/author/crx-quickstart/bin
RUN mv quickstart quickstart.original
ADD resources/quickstart /aem/author/crx-quickstart/bin/quickstart
RUN chmod +x /aem/author/crx-quickstart/bin/quickstart
RUN chmod +x /aem/author/compaction.sh

EXPOSE 4502
ENTRYPOINT ["/aem/author/crx-quickstart/bin/quickstart"]
#ENTRYPOINT ["/aem/author/compaction.sh"]
