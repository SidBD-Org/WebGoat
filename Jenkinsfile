stage('Polaris SCA Scan') {
    steps {
        sh '''
        set -euo pipefail

        echo "Downloading Bridge CLI bundle..."
        curl -fLsS -o bridge.zip "$BRIDGECLI_URL"

        echo "Unzipping..."
        rm -rf bridgecli && mkdir -p bridgecli
        unzip -qo bridge.zip -d bridgecli

        echo "Locating bridge-cli binary..."
        BRIDGE_BIN="$(find bridgecli -type f -name bridge-cli -perm -u+x | head -n 1)"
        if [ -z "$BRIDGE_BIN" ]; then
          echo "ERROR: bridge-cli binary not found after unzip"; ls -R bridgecli; exit 1
        fi
        echo "bridge-cli found at: $BRIDGE_BIN"

        echo "Running Polaris SCA (package + signature)..."
        "$BRIDGE_BIN" --stage polaris \
          polaris.serverUrl="$POLARIS_SERVER_URL" \
          polaris.application.name="$POLARIS_APPLICATION_NAME" \
          polaris.project.name="$POLARIS_PROJECT_NAME" \
          polaris.branch.name="$POLARIS_BRANCH_NAME" \
          polaris.assessment.types=SCA \
          polaris.sca.types=SCA-PACKAGE,SCA-SIGNATURE
        '''
    }
}
