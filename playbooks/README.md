# Which files to use?

Only `master-base-playbook.yaml` and `node-base-playbook.yaml` are used in the Vagrant file.
These contain (as of July 13th 2022) Kubernetes v1.24 running with containerd as CRI (because Docker is not officially supported by Kubernetes anymore). However, they still include the Docker binaries (very practical if you want to build/push images locally).
The other files you see here are backup for other legacy versions of Kubernetes that we used before. I don't recommend using them or editing them.
