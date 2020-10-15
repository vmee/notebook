# IAM


## Top 10 AWS IAM BEST Practices

- users: create individual users
- permissions: Grant least priviledge
- groups: manage permissions with groups
- conditions: restrict priviledged access further with conditions
- auditing: enable cloudTrail to get logs of API calls
- password: configure a strong password policy
- rotate: rotate security credentials regularly.
- MFA: enable MFA (Multi-Factor Authentication) for priviledged users
- sharing: use IAM roles to share access
- roles: use IAM roles for EC2 instances
- root: reduce or remove use of root


以上是policy的一些基础用法，下面讲讲policy的执行规则，它也是几乎所有访问控制方案的通用规则：

- 默认情况下，一切资源的一切行为的访问都是Deny
- 如果在policy里显式Deny，则最终的结果是Deny
- 否则，如果在policy里是Allow，则最终结果是Allow
- 否则，最终结果是Deny


## 参考资料

[https://blog.csdn.net/whatnamecaniuse/article/details/78104189](https://blog.csdn.net/whatnamecaniuse/article/details/78104189)

[https://www.jianshu.com/p/f59745ae7fad?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation](https://www.jianshu.com/p/f59745ae7fad?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

[https://docs.aws.amazon.com/zh_cn/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_Boolean](https://docs.aws.amazon.com/zh_cn/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_Boolean)

