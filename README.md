Using Parameters from one child generator in another child generator

https://argo-cd.readthedocs.io/en/stable/operator-manual/applicationset/Generators-Matrix/#using-parameters-from-one-child-generator-in-another-child-generator

```
ApplicationSet:
  for file in files:
    list_items=$(open $file)
    for item in list_items:
      create Application
        with item.values_file.values
```

https://argocd-applicationset.readthedocs.io/en/stable/Generators-Git/#git-generator-files
https://argo-cd.readthedocs.io/en/stable/operator-manual/applicationset/Generators-Git/#git-generator-files

https://argo-cd.readthedocs.io/en/stable/user-guide/helm/#helm-value-precedence

## Hot to create a list generator from elements in a yaml/json file

https://argo-cd.readthedocs.io/en/stable/operator-manual/applicationset/Generators-List/#dynamically-generated-elements

## Value files path can be full git path or relative

Sources:

- https://argo-cd.readthedocs.io/en/stable/user-guide/helm/#values-files
- https://argo-cd.readthedocs.io/en/stable/user-guide/multiple_sources/#helm-value-files-from-external-git-repository
