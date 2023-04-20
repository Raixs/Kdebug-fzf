
Kubefzf-Debugger es una herramienta interactiva que permite seleccionar y depurar contenedores de un clúster de Kubernetes utilizando fzf y kubectl debug.

## Requisitos

-   [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
-   [fzf](https://github.com/junegunn/fzf)

## Instalación

1.  Clone este repositorio:

`git clone https://github.com/raixs/kdebug-fzf`

2.  Cambie al directorio clonado:

`cd kdebug-fzf`

3.  Haga que el script sea ejecutable:

`chmod +x kdebug-fzf.sh

4.  Opcionalmente, puede agregar el directorio del script a su variable de entorno `PATH` para ejecutarlo desde cualquier lugar.

## Uso

5. Opcionalmente, puedes crear un alias para el script:

`alias kdebug="~/kdebug-fzf/kdebug-fzf.sh"`

Para usar Kdebug-fzf, ejecute el script `kdebug-fzf`:

`./kdebuf-fzf.sh`

1.  Seleccione un namespace de la lista desplegable.
2.  Seleccione un pod del namespace seleccionado.
3.  Seleccione un contenedor del pod seleccionado.

El script iniciará una sesión de depuración en el contenedor seleccionado utilizando `kubectl debug` y la imagen de `busybox`.