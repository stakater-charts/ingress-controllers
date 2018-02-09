#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String internalIngressChartPackageName = ""
String externalIngressChartPackageName = ""
String internalIngressChartName = "internal-ingress"
String externalIngressChartName = "external-ingress"

clientsNode(clientsImage: 'stakater/pipeline-tools:dev') {
    container(name: 'clients') {
        def helm = new io.stakater.charts.Helm()
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
            chartManager.uploadToChartMuseum(WORKSPACE, internalIngressChartName, internalIngressChartPackageName)
            chartManager.uploadToChartMuseum(WORKSPACE, externalIngressChartName, externalIngressChartPackageName)
        }
    }
}
