FROM centos:latest

## This handle reaches Marcel van den Bosch
MAINTAINER "Marcel van den Bosch" marcel.vandenbosch@atos.net

## Add EPEL repo
RUN yum install -y epel-release

# Refresh
RUN yum update -y

# Install R
RUN yum install -y git wget libcurl-devel libpng-devel libjpeg-devel vim R-core R-core-devel libxml2-devel gtk2-devel v8-devel

# R / Shiny specifics
RUN R -e "install.packages(c('shiny', 'rmarkdown'), repos='https://cran.rstudio.com/')"
RUN R -e "install.packages(c('shiny','tools','shinydashboard','shinyjs','data.table','DT','rpart','rattle','randomForest','sampling','e1071','caTools','sm','pastecs','DMwR','corrplot','caret','xgboost','V8','ggplot2','assertthat','rpart.plot'),repos='http://cran.us.r-project.org')"

RUN wget sudo wget -O rstudio-latest-x86_64.rpm https://www.rstudio.org/download/latest/stable/desktop/redhat64/rstudio-latest-x86_64.rpm && yum install -y --nogpgcheck shiny-server-1.5.3.838-rh5-x86_64.rpm && yum install --nogpgcheck rstudio-latest-x86_64.rpm

RUN yum clean all

RUN cp -R /usr/lib64/R/library/shiny/examples/* /srv/shiny-server/

# Copy shiny-server launcher file
COPY shiny-server.sh /usr/bin/shiny-server.sh
RUN chmod +x /usr/bin/shiny-server.sh

WORKDIR /srv/shiny-server

# Clone git repo into shiny server target directory
RUN mkdir -p /srv/shiny-server && git clone https://a664091@bitbucket.fsc.atos-services.net/scm/cfl/codex-shared-fab-lab-repo.git


EXPOSE 3838

# Execute Shiny server
CMD ["/usr/bin/shiny-server.sh"]
