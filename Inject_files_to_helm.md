Using Helm's `tpl` function in combination with Go template functions offers a flexible way to inject file content into your Helm chart based on paths specified in the `values.yaml` file. This method involves two steps: specifying file paths in `values.yaml`, and then using a template file to read and inject the content of these files into the Kubernetes manifests dynamically.

### Step 1: Specify File Paths in `values.yaml`

First, list the paths to the Kubernetes resource files you want to include in your Helm chart within the `values.yaml` file. For example:

```yaml
# values.yaml
resourceFilePaths:
  - ./resources/pvc.yaml
  - ./resources/configmap.yaml
```

### Step 2: Create a Template to Read and Inject File Content

Next, create a template file in the `templates` directory of your Helm chart that reads and injects the content of the specified files. You can use Helm's `tpl` function along with the `Files.Get` function to achieve this. The `tpl` function allows you to render strings as templates, which is powerful when combined with file contents.

Here’s an example template, `inject-resources.yaml`, that does this:

```yaml
{{- range .Values.resourceFilePaths }}
---
{{- $filePath := . }}
{{- with $.Files.Get $filePath }}
{{- tpl . $ | nindent 0 }}
{{- end }}
{{- end }}
```

In this template:
- We iterate over each file path specified in `resourceFilePaths`.
- For each path, we retrieve the file content using `$.Files.Get`.
- We then use `tpl` to render the file content as a template. This allows any Go template directives in the file content to be processed. The `tpl` function's second argument, `$`, passes the current context to the template, making values from `values.yaml` and chart helpers available.
- The `nindent 0` function is used to ensure the formatting is preserved correctly.

### Important Notes

- **Files Location**: The files referenced in `values.yaml` must be located within the Helm chart directory, or more specifically, under the chart's root where the `Chart.yaml` file is located. Helm includes these files in the chart package.

- **Security and Complexity**: This approach allows for significant flexibility but comes with potential security and complexity concerns. Be cautious with template rendering, especially if file content or paths might come from untrusted sources.

- **Files in .helmignore**: Ensure that the files you want to include are not listed in `.helmignore`. Files listed in `.helmignore` are not packaged with the chart, and therefore, `Files.Get` won't be able to access them.

Using the `tpl` function in this way allows you to dynamically include and process Kubernetes resource files based on paths specified in `values.yaml`, providing a powerful mechanism to extend and customize your Helm chart's deployed resources.
