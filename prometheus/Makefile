CHART_REPO_NAME   := kube-state-metrics
CHART_REPO_URL    := https://kubernetes.github.io/kube-state-metrics
CHART_NAME        := prometheus
CHART_VERSION     := 14.1.0
CHART_VALUES_FILE := values.yaml

add-chart-repo:
    helm repo add ${CHART_REPO_NAME} ${CHART_REPO_URL}
    helm repo update

generate-chart-manifest: add-chart-repo
    helm template ${CHART_NAME} ${CHART_REPO_NAME}/${CHART_NAME} \
        --version ${CHART_VERSION} \
        --values ${CHART_VALUES_FILE} > ${CHART_NAME}.yaml

get-chart-values:
    @helm show values --version ${CHART_VERSION} \
    ${CHART_REPO_NAME}/${CHART_NAME}

generate-chart-values:
    @echo "Create values file: ${CHART_VALUES_FILE}"
    @$(MAKE) -s get-chart-values > ${CHART_VALUES_FILE}

diff-chart-values:
    @echo "Diff: Local <==> Remote"
    @$(MAKE) -s get-chart-values | \
    diff --suppress-common-lines --side-by-side ${CHART_VALUES_FILE} - || \
    exit 0