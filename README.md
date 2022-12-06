[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]

# Knative Serving Raspberry Pi

- Knative Serving
- Kourier
- SSLIP.IO

## | [Prerequisites](#prerequisites) | [Quick Start](#quick-start) | [See Also](#see-also) | [Contributing](#contributing) | [License](#license) |

## Prerequisites

- Raspberry Pi running Ubuntu Server 64-bit
  - Kubernetes without CNI installed

## Quick Start

```bash
git clone https://github.com/dashaun/knative-serving-raspberry-pi
cd knative-serving-raspberry-pi/operator-serving-kourier
# Use the `default` namespace in the Kubernetes cluster
kubectl config set-context --current --namespace=default
# Deploy the Knative operator
kubectl apply -f operator.yaml
# Check the deployment status
kubectl get deployment knative-operator
```
> Continue when `Ready` status is `1/1`
> ```
> NAME               READY   UP-TO-DATE   AVAILABLE   AGE
> knative-operator   1/1     1            1           19h
> ```
```bash
# Install Knative Serving custom resource and Kourier
kubectl apply -f knative-serving.yaml
# Check the custom resource status
kubectl get KnativeServing knative-serving -n knative-serving
```
> Continue when `READY` is `True`
> ```text
> NAME              VERSION   READY   REASON
> knative-serving   1.8.0     True
> ```
```bash
# Check the deployment status
kubectl get deployment -n knative-serving
```
> Continue when `READY` are all `1/1`
> ```text
> NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
> activator                1/1     1            1           23m
> autoscaler               1/1     1            1           23m
> controller               1/1     1            1           23m
> domain-mapping           1/1     1            1           23m
> domainmapping-webhook    1/1     1            1           23m
> autoscaler-hpa           1/1     1            1           23m
> 3scale-kourier-gateway   1/1     1            1           23m
> webhook                  1/1     1            1           23m
> net-kourier-controller   1/1     1            1           23m
> ```

```bash
# Install default domain
kubectl apply -f serving-default-domain.yaml
# Deploy a Spring Boot 3 `native` application
kubectl apply -f spring-boot-native-pi-service.yaml
# Get the address
kubectl get ksvc spring-boot-native-pi --output=custom-columns=NAME:.metadata.name,URL:.status.url
```
>```text
>NAME                    URL
>spring-boot-native-pi   http://spring-boot-native-pi.default.10.0.0.10.sslip.io
>```

```bash
# Curl the address provided by knative with `/actuator/health` at the end
curl http://spring-boot-native-pi.default.default.10.0.0.10.sslip.io/actuator/health
```
> Expected output:
>```text
>{"status":"UP"}
>```

## See Also
- [spring-boot-native-pi](https://github.com/dashaun/spring-boot-native-pi)
- [K3s KNative Ubuntu Raspberry Pi](https://dashaun.com/posts/)

<!-- CONTRIBUTING -->
## Contributing

Pull-requests are welcomed!

<!-- LICENSE -->
## License

Distributed under the Apache License, Version 2.0. See `LICENSE` for more information.

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/dashaun/knative-serving-raspberry-pi.svg?style=for-the-badge
[contributors-url]: https://github.com/dashaun/knative-serving-raspberry-pi/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/dashaun/knative-serving-raspberry-pi.svg?style=for-the-badge
[forks-url]: https://github.com/dashaun/knative-serving-raspberry-pi/network/members
[stars-shield]: https://img.shields.io/github/stars/dashaun/knative-serving-raspberry-pi.svg?style=for-the-badge
[stars-url]: https://github.com/dashaun/knative-serving-raspberry-pi/stargazers
[issues-shield]: https://img.shields.io/github/issues/dashaun/knative-serving-raspberry-pi.svg?style=for-the-badge
[issues-url]: https://github.com/dashaun/knative-serving-raspberry-pi/issues
[license-shield]: https://img.shields.io/github/license/dashaun/knative-serving-raspberry-pi.svg?style=for-the-badge
[license-url]: https://github.com/dashaun/knative-serving-raspberry-pi/blob/master/LICENSE.txt