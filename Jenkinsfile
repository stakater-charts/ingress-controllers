#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String internalIngressChartPackageName = ""
String externalIngressChartPackageName = ""
String internalIngressChartName = "internal-ingress"
String externalIngressChartName = "external-ingress"

toolsNode(toolsImage: 'stakater/pipeline-tools:1.2.0') {
    container(name: 'tools') {
        def helm = new io.stakater.charts.Helm()
        def common = new io.stakater.Common()
        def chartManager = new io.stakater.charts.ChartManager()
        stage('Checkout') {
            checkout scm
        }
        
        stage('Init Helm') {
            helm.init(true)
        }

        stage('Prepare Chart') {
            helm.lint(WORKSPACE, internalIngressChartName)
            internalIngressChartPackageName = helm.package(WORKSPACE, internalIngressChartName)

            helm.lint(WORKSPACE, externalIngressChartName)
            externalIngressChartPackageName = helm.package(WORKSPACE, externalIngressChartName)
        }

        stage('Upload Chart') {
            String cmUsername = common.getEnvValue('CHARTMUSEUM_USERNAME')
            String cmPassword = common.getEnvValue('CHARTMUSEUM_PASSWORD')
            chartManager.uploadToChartMuseum(WORKSPACE, internalIngressChartName, internalIngressChartPackageName, cmUsername, cmPassword)
            chartManager.uploadToChartMuseum(WORKSPACE, externalIngressChartName, externalIngressChartPackageName, cmUsername, cmPassword)
        }
    }
}
