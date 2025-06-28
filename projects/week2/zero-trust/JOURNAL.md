# Journal

## Zero-Trust System

Developed by Google and called **BeyondCorp**.

What it is: BeyondCorp is Google’s implementation of a Zero-Trust security
model, which assumes that no network (internal or external) is inherently
trusted. Instead of relying on perimeter-based security (e.g., VPNs), it
enforces strict identity and device verification before granting access to
resources, regardless of where the user is located.

Why it is used:

- Traditional security assumes that anything inside the corporate network is
  safe, but this breaks down with remote work, cloud services, and sophisticated
  attacks.
- BeyondCorp improves security by requiring:

  - Strong user authentication (who you are)
  - Device trust (is your device safe and compliant?)
  - Context-aware policies (e.g., location, time, risk signals)

- It reduces attack surfaces, eliminates reliance on VPNs, and better protects
      against insider threats, phishing, and lateral movement by attackers

## Policy Engine

A policy engine is a system that evaluates whether a particular action should be
allowed or denied, based on a set of defined policies. These policies are
typically expressed as rules involving users, devices, contexts, and resources.

Example:

- Common policy engines include:

  - Open Policy Agent (OPA) – cloud-native, general-purpose
  - AWS IAM Policy Engine – cloud permissions
  - Kubernetes Admission Controllers

## Pomeranium

Pomerium is an open-source identity-aware proxy (IAP) designed for implementing
Zero-Trust security models for internal web applications.

Example of What You Might Use Pomerium For:

- Internal developer tools (e.g., Jenkins, GitLab) accessible securely over the
  internet.
- Replace VPN access to web services.
- Implement least-privilege access policies for sensitive internal applications.
