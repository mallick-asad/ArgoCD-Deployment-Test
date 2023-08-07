Example configuration of eks-argocd to deploy the root app:





module "eks-argocd" {
  source           = "https://modules.gdo.numerator.cloud/eks-argocd/0.X.X"
  name             = "myargocd"
  namespace        = "argocd"
  create_namespace = true
  .
  .
  .
  argocd_apps_enabled        = true
  argocd_apps_helm_values    = <<-EOF
    applications:
      - name: app-of-apps
        namespace: argocd # NOTE: This must be the namespace where your eks-argocd installation lives!
        project: default
        source:
          repoURL: https://github.com/mallick-asad/ArgoCD-Deployment-Test
          targetRevision: HEAD
          path: app-of-apps/rootapp
        destination:
          server: https://kubernetes.default.svc
        syncPolicy:
          automated:
            prune: true # Automatic sync will not delete resources if false
            selfHeal: true
  EOF
