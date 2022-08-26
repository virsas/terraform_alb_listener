# terraform_alb_listener_ssl

Terraform module to create AWS loadbalancer listener with SSL policy attached

##  Dependencies

ALB - <https://github.com/virsas/terraform_alb>

## Files

- None

## Terraform example

``` terraform
##############
# Variable
##############
variable "alb_listener_main_https" {
  default = {
    # port to listen to
    port = "443"
    # protocol to listen to
    protocol = "HTTPS"
    # policy used (limited to tls 1.2 below)
    policy = "ELBSecurityPolicy-TLS-1-2-2017-01"
    # if no rule is satisfied, default action will be executed. In this case redirect to example.org website
    default_action_redirect_protocol  = "HTTPS"
    default_action_redirect_host      = "example.org"
    default_action_redirect_port      = "443"
    default_action_redirect_path      = "/#{path}"
    default_action_redirect_query     = "#{query}"
  }
}

##############
# Module
##############
module "alb_listener_main_https" {
  source = "git::https://github.com/virsas/terraform_alb_listener_ssl.git?ref=v1.0.0"
  listener = var.alb_listener_main_https
  acm = module.acm_alb_eu.arn
  alb = module.alb_main.arn
}
```

## Outputs

- arn