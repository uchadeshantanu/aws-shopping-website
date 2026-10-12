# Architecture Notes

## Request flow

1. The browser sends an HTTP request to the ALB on port 80.
2. The ALB evaluates its listener rules.
3. `/cloth` and `/cloth/*` are forwarded to `cloth-tg`.
4. `/laptop` and `/laptop/*` are forwarded to `laptop-tg`.
5. Unmatched requests use the default action and are forwarded to `home-tg`.
6. The target group forwards the request to its registered EC2 backend, where Nginx serves static HTML.

## Components

- One internet-facing Application Load Balancer
- One HTTP listener on port 80
- Three target groups
- Separate EC2/Nginx backends for Home, Cloth, and Laptop
- Three launch templates and three Auto Scaling groups

An additional instance launch was reported. Confirm the ASG Activity history, the instance's `InService` state, and target-group health before marking scale-out fully verified. This does not automatically verify replacement or scale-in.
