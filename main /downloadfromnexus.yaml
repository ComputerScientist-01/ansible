SH_NAME="DownloadFromNexus"

########################## Download from Nexus
nexusDir=https://nexus305.systems.uk.hsbc:8081/nexus/repository/maven-hsbc-internal-dev_n3p/com/hsbc/gbm/financeit/corb3/release
nexusProdDir=https://nexus305.systems.uk.hsbc:8081/nexus/repository/maven-hsbc-internal-prod_n3p/com/hsbc/gbm/financeit/corb3/release #PromoteToProd

if [ $# -ne 1 ]; then
   echo "Please pass artifact package name, e.g. REGCORB3-1.11.5"
   exit 1
fi

########################## Validate Credentials
if [ -z "$NEXUS_USERNAME" ] || [ -z "$NEXUS_PASSWORD" ]; then
  echo "Nexus credentials are not set in the environment. Exiting..."
  exit 1
fi

########################## Download from Nexus
echo "Downloading package from NEXUS"

BUILD_NAME=$1
curl -k -u "$NEXUS_USERNAME:$NEXUS_PASSWORD" -l "$nexusDir/" | grep "._v" > temp.txt
cat temp.txt | cut -d'"' -f3 | sed 's/.tar.gz.*//g' | sed 's/<.*//g' | sed 's/>//g' > nexusList.txt
var=$(cat nexusList.txt | grep -w ${BUILD_NAME} | sort -u)

# PromoteToProd start
curl -k -u "$NEXUS_USERNAME:$NEXUS_PASSWORD" -l "$nexusProdDir/" | grep "._v" > Prodtemp.txt
cat Prodtemp.txt | cut -d'"' -f3 | sed 's/.tar.gz.*//g' | sed 's/<.*//g' | sed 's/>//g' > ProdnexusList.txt
Prodvar=$(cat ProdnexusList.txt | grep -w ${BUILD_NAME} | sort -u)
# PromoteToProd end

if [ "$var" == "${BUILD_NAME}" ]; then
   echo ""
   echo "$var found over Nexus..."
   echo "Downloading ${BUILD_NAME} package from Nexus $nexusDir"
   curl -k --silent -u "$NEXUS_USERNAME:$NEXUS_PASSWORD" -O "$nexusDir/${BUILD_NAME}/1.0.0/${BUILD_NAME}-1.0.0.tar.gz"

   if [ $? -ne 0 ]; then
      echo "Download Failed... Please check on Nexus"
      exit 1
   fi

   echo "Downloaded ${BUILD_NAME} successfully from Nexus $nexusDir"
elif [ "$Prodvar" == "${BUILD_NAME}" ]; then
   echo ""
   echo "$Prodvar found over Nexus..."
   echo "Downloading ${BUILD_NAME} package from Nexus $nexusProdDir"
   curl -k --silent -u "$NEXUS_USERNAME:$NEXUS_PASSWORD" -O "$nexusProdDir/${BUILD_NAME}/1.0.0/${BUILD_NAME}-1.0.0.tar.gz"
   
   if [ $? -ne 0 ]; then
      echo "Download Failed... Please check on Nexus"
      exit 1
   fi
   
   echo "Downloaded ${BUILD_NAME} successfully from Nexus $nexusProdDir"
else
   echo ""
   echo "${BUILD_NAME} package NOT FOUND on Nexus... Please check on Nexus"
fi
########################## End of Download from Nexus
