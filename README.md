
---

# 🛡️ Kyverno Default-Deny NetworkPolicy Generator

This Kyverno `ClusterPolicy` automatically creates a **default deny-all `NetworkPolicy`** in any new **Namespace**.
It helps enforce a **zero-trust** model by **blocking all ingress and egress traffic** unless explicitly allowed.

---

## 📋 Policy Description

* Kubernetes by default allows unrestricted communication between all Pods across Namespaces.
* This policy ensures that **each new Namespace** automatically gets a `NetworkPolicy` called `default-deny` which:

  * **Selects all Pods** (`podSelector: {}`).
  * **Denies all Ingress and Egress traffic**.
* Application teams must define additional `NetworkPolicy` resources to allow specific traffic as needed.
* This improves cluster security and aligns with **EKS Best Practices** and **multi-tenancy models**.


---

## 🚀 How to Apply This Policy

Follow these steps to implement the Kyverno policy:

1. **Ensure Kyverno is installed in your cluster**:

   If Kyverno is not already installed, you can install it using:

   ```bash
   kubectl create -f https://raw.githubusercontent.com/kyverno/kyverno/main/config/install.yaml
   ```

2. **Apply the ClusterPolicy**:

   Save the policy YAML to a file (e.g., `add-networkpolicy.yaml`) and apply it:

   ```bash
   kubectl apply -f add-networkpolicy.yaml
   ```

3. **Verify that the policy is active**:

   ```bash
   kubectl get clusterpolicy
   ```

   You should see `add-networkpolicy` listed.

4. **Test the Policy**:

   * Create a new Namespace:

     ```bash
     kubectl create namespace test-ns
     ```

   * Check if a `NetworkPolicy` named `default-deny` was automatically created inside `test-ns`:

     ```bash
     kubectl get networkpolicy -n test-ns
     ```

     Expected output:

     ```bash
     NAME           POD-SELECTOR   AGE
     default-deny   <all pods>     <seconds>
     ```

5. **(Optional) Allow Specific Traffic**:

   After applying the default deny policy, application teams must create additional `NetworkPolicy` resources to explicitly allow necessary communications.

---

## 📢 Important Notes

* Your Kubernetes cluster must have a **CNI plugin** that supports **NetworkPolicy** enforcement, such as:

  * Calico
  * Cilium
  * AWS VPC CNI with Policy Enforcement
* Without a compatible CNI, the `NetworkPolicy` objects will exist but will **not be enforced**.
* The `synchronize: true` option ensures that the generated `NetworkPolicy` stays updated if the policy itself changes.

---

# 📚 References

* [Kyverno Documentation](https://kyverno.io/docs/)
* [Kubernetes Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
* [EKS Best Practices Guides](https://aws.github.io/aws-eks-best-practices/networking/network-policies/)

---

# ✍️ Example README Flow:

| Section            | Purpose                                            |
| :----------------- | :------------------------------------------------- |
| Project Title      | "Kyverno Default-Deny NetworkPolicy"               |
| Description        | Short explanation of what problem it solves        |
| Policy YAML        | Full YAML code block                               |
| Steps to Implement | Clear install and apply steps                      |
| Test Instructions  | Create namespace, see auto-generated NetworkPolicy |
| Important Notes    | CNI support required                               |
| References         | Useful links                                       |

---

Would you like me to also create a **sample folder structure** for your GitHub project to look even more professional? 📁 (like `policies/`, `readme/`, `examples/`) — it can make your project look top-notch! 🚀
