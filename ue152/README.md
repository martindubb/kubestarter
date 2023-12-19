# Fragen

- Gib den entsprechenden Befehl ein, um deinen lokalen Kubernetes-Cluster zu starten. Wie lautet der Befehl?
  - `minikube start`

- Nach erfolgreichem Start überprüfe den Status des Clusters. Wie lautet der Befehl dafür?
  - `minikube status`

- Starte einen Pod mit dem Name "my-pod", welcher das Nginx-Image verwendet. Verwende dazu kubectl run und gib den kompletten Befehl hier an.
  - `kubectl run my-pod --image=nginx`

- Wie kann der Status des Pods überprüft werden?
  - `kubectl get pods`
  - `kubectl describe pod my-pod`

- Wie kann die Konfiguration des Pods im  YAML-Format ausgegeben werden?
  - `kubectl get pod my-pod -oyaml`
  - `kubectl get pod my-pod -oyaml > my-pod.yaml`

- Bearbeite die YAML-Datei, die du gerade exportiert hast, und ändere das Image von nginx zu httpd.

    ```yaml
    [...]
    spec:
      containers:
      - image: httpd            # <---- änderung
        imagePullPolicy: Always
        name: my-pod
        resources: {}
    [...]
    ```

- Wende die Änderungen mit kubectl apply an. Wie lautet der komplette Befehl?
  - `kubectl apply -f my-pod.yaml`

- Bearbeite erneut die YAML-Datei und füge unter einen weiteren Container hinzu mit dem Namen "redis-container" und dem Image redis.

    ```yaml
    [...]
    spec:
      containers:
      - image: httpd
        imagePullPolicy: Always
        name: my-pod
        resources: {}
      - image: redis                # <---- änderung
        imagePullPolicy: Always     # <---- änderung
        name: redis                 # <---- änderung
        resources: {}               # <---- änderung
    [...]
    ```

- Wende die Änderungen erneut mit kubectl apply an.
  > Es erscheint die Fehlermeldung: `The Pod "my-pod" is invalid: spec.containers: Forbidden: pod updates may not add or remove containers`
  > Es dürfen keine Container zu laufenden Pods hinzugefügt bzw. entfernt werden! *Lösung: Pod löschen und neu erstellen!*
  
  `kubectl delete pod my-pod && kubectl apply -f my-pod.yaml`

- Wie kann verifiziert werden, dass ein zweiter Pod mit den korrekten Spezifikationen läuft?
  - nicht korrekt: `kubectl get pods` -> man sieht hier, dass 2 container laufen (`2/2`), allerdings sagt uns das nichts über die Spezifikation!
  - entweder `kubectl describe pod my-pod` oder `kubectl get pods -oyaml`
