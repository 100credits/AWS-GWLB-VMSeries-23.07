# AWS-GWLB-VMSeries-23.07
Modified version from "aws_subnet_ids" to "aws_subnets".

app_stack > alb.tf
resource "aws_alb" "alb" {
 name      = "app-alb-${random_id.deployment_id.hex}"
 subnets    = data.aws_subnets.alb_subnet_ids.ids
 security_groups = [aws_security_group.app-sg.id]
 internal    = false
 tags = {
  Name  = "app-alb-${random_id.deployment_id.hex}"
 }

app_stack > network.tf
data "aws_subnets" "alb_subnet_ids" {
 filter {
 name = "vpc-id"
 values = [aws_vpc.app_vpc.id]
}
 tags = {
  UsedBy = "ALB"
 }
 depends_on = [aws_subnet.alb_subnet]
}
