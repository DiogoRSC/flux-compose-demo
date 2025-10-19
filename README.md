# Flux Compose Demo

This repo provides a demonstration of using a Compose file to define an application running in Kubernetes (inside Docker) and deployed using Flux. It is making use of my [compose-deployer Helm chart](https://github.com/mikesir87/helm-charts/charts/compose-deployer), but it can easily be swapped out with any other tooling.

## Running it Yourself

If you want to exercise the full demo, you will want to do the following:

1. Install flux

   ```shell
   choco install flux
   ```

2. If you need Traefik/Flux to be installed, you can do so using the manifests in the `/pre-reqs` directory:

   ```shell
   kubectl apply -k ./pre-reqs
   ```

3. Start the application:

   ```shell
   kubectl apply -f ./flux
   ```

4. Get pods

   ```shell
   kubectl get pods
   ```

   You should be able to open [http://vote.localhost](http://vote.localhost) and see the voting app. Opening [http://results.localhost](http://results.localhost) should give you the results page (Chrome auto-resolves \*.localhost to localhost, so hopefully it works by default for you).

5. Make a change to the `docker-compose.yml` file. A good idea might be is to define the `OPTION_A` and `OPTION_B` environment variables on the voting app to change what you're voting for. If you push the file, you should see a pipeline get triggered and the app be deployed on your local machine within a moment or two.

### Cleaning up

When you're ready to tear everything down, simply remove the flux config, which will then remove all of the apps:

```shell
kubectl delete -f ./flux
```

And if you want to remove the Traefik ingress and Flux components, you can do so using this command:

```shell
kubectl delete -k ./pre-reqs
```
