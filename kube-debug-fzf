#!/bin/bash

# Primero, obtenemos una lista de namespaces en el clúster.
namespaces=$(kubectl get namespaces --no-headers -o custom-columns=":metadata.name")

# Luego, usamos fzf para seleccionar interactivamente un namespace y mostrar los pods en una ventana lateral.
selected_namespace=$(echo "${namespaces}" | fzf --prompt "Selecciona un namespace: " --preview "echo 'Pods en el namespace:' && kubectl get pods --namespace {} --no-headers" --preview-window "right:60%:wrap")

# Verificamos si el usuario ha seleccionado un namespace.
if [ -z "${selected_namespace}" ]; then
  echo "No se seleccionó ningún namespace."
  exit 1
fi

# Obtenemos una lista de pods en el namespace seleccionado.
pods=$(kubectl get pods --namespace "${selected_namespace}" --no-headers -o custom-columns=":metadata.name")

# Luego, usamos fzf para seleccionar interactivamente un pod y mostrar los contenedores en una ventana lateral.
selected_pod=$(echo "${pods}" | fzf --prompt "Selecciona un pod: " --preview "echo 'Contenedores en el pod:' && kubectl get pods {} --namespace '${selected_namespace}' -o jsonpath='{.spec.containers[*].name}' | tr ' ' '\n'" --preview-window "right:60%:wrap")

# Verificamos si el usuario ha seleccionado un pod.
if [ -z "${selected_pod}" ]; then
  echo "No se seleccionó ningún pod."
  exit 1
fi

# A continuación, obtenemos una lista de contenedores en el pod seleccionado.
containers=$(kubectl get pods "${selected_pod}" --namespace "${selected_namespace}" -o jsonpath='{.spec.containers[*].name}')

# Usamos fzf para seleccionar interactivamente un contenedor dentro del pod.
selected_container=$(echo "${containers}" | tr ' ' '\n' | fzf --prompt "Selecciona un contenedor: ")

# Verificamos si el usuario ha seleccionado un contenedor.
if [ -z "$selected_container" ]; then
  echo "No se seleccionó ningún contenedor."
  exit 1
fi

#Obtenemos un timestamp para añadir al nombre del pod
timestamp=$(date +%s)

# Creamos un nombre único para el contenedor de depuración agregando un sufijo al nombre del contenedor seleccionado.
debug_container_name="${selected_container}-debug-${timestamp}"

# Finalmente, usamos kubectl debug para conectarnos al contenedor seleccionado en el namespace seleccionado.
kubectl debug -it "${selected_pod}" --namespace "${selected_namespace}" --image=busybox --container="${debug_container_name}" --target="${selected_container}" -- sh
