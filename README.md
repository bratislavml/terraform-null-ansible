## terraform-null-ansible
<!---
  --- This file was automatically generated by the `build-harness`
  --- Make changes instead to `.README.md` and rebuild.
  --->

Terraform Module to run ansible playbooks.

## Module usage

### Add special section to the playbook

You must add this section on the top of all playbooks that will be used for provisioning.

This will add a dynamic inventory to target the host that needs provisioning.

e.g. `../ansible/playbooks/playbook.yml`

* Create a runtime inventorty with an ip address of a host
* Wait for target host is ready for ssh connection

```yaml

---
- hosts: localhost
  gather_facts: True
  check_mode: no
  tasks:
  - name: Add public ip addresses to an dynamic inventory
    add_host:
      name: "{{ host }}"
      groups: all

  - local_action: wait_for port=22 host="{{ host }}" search_regex=OpenSSH delay=10

- hosts: all
  gather_facts: False
  check_mode: no
  become: True
  tasks:
  - name: Install python 2.7
    raw: >
      test -e /usr/bin/python ||
      (
        (test -e /usr/bin/apt-get && (apt-get -y update && apt-get install -y python)) ||
        (test -e /usr/bin/yum && (yum makecache fast && yum install -y python))
      )
    args:
      creates: /usr/bin/python
```

### Create an aws instance

```hcl
resource "aws_instance" "web" {
  ami           = "ami-408c7f28"
  instance_type = "t1.micro"
  tags {
    Name        = test1
  }
}
```

### Apply the provisioner module to this resource

```hcl
module "ansible_provisioner" {
   source    = "github.com/cloudposse/tf_ansible"

   arguments = ["--user=ubuntu"]
   envs      = ["host=${aws_instance.web.public_ip}"]
   playbook  = "../ansible/playbooks/test.yml"
   dry_run   = false

}
```

## Input

<!--------------------------------REQUIRE POSTPROCESSING-------------------------------->
|  Name |  Default  |  Description  |
|:------|:---------:|:--------------:|
| arguments |[] |Arguments|
| dry_run |"true" |Do dry run|
| envs |[] |Environment variables|
| playbook |"" |Playbook to run|

## Output

<!--------------------------------REQUIRE POSTPROCESSING-------------------------------->
|  Name | Description  |
|:------|:------------:|
| arguments | Arguments  |
| envs | Environment variables  |

## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/terraform-null-ansible/issues), send us an [email](mailto:hello@cloudposse.com) or reach out to us on [Gitter](https://gitter.im/cloudposse/).

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-null-ansible/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing `terraform-null-ansible`, we would love to hear from you! Shoot us an [email](mailto:hello@cloudposse.com).

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull request** so that we can review your changes

**NOTE:** Be sure to merge the latest from "upstream" before making a pull request!

## License

[APACHE 2.0](LICENSE) © 2017 [Cloud Posse, LLC](https://cloudposse.com)

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know at <hello@cloudposse.com>

We love [Open Source Software](https://github.com/cloudposse/)!

See [our other projects][community]
or [hire us][hire] to help build your next cloud-platform.

  [website]: http://cloudposse.com/
  [community]: https://github.com/cloudposse/
  [hire]: http://cloudposse.com/contact/

### Contributors

|[![Erik Osterman][erik_img]][erik_web]<br/>[Erik Osterman][erik_web] |[![Sergey Vasilyev][sergey_img]][sergey_web]<br/>[Sergey Vasilyev][sergey_web] |[![Vladimir][vladimir_img]][vladimir_web]<br/>[Vladimir][vladimir_web] |[![Igor Rodionov][igor_img]][igor_web]<br/>[Igor Rodionov][igor_img] |[![Konstantin B][konstantin_img]][konstantin_web]<br/>[Konstantin B][konstantin_web] |

|---|---|---|---|---|

[andriy_img]: https://avatars0.githubusercontent.com/u/7356997?v=4&u=ed9ce1c9151d552d985bdf5546772e14ef7ab617&s=144
[andriy_web]: https://github.com/aknysh/

[erik_img]: http://s.gravatar.com/avatar/88c480d4f73b813904e00a5695a454cb?s=144
[erik_web]: https://github.com/osterman/

[igor_img]: http://s.gravatar.com/avatar/bc70834d32ed4517568a1feb0b9be7e2?s=144
[igor_web]: https://github.com/goruha/

[konstantin_img]: https://avatars1.githubusercontent.com/u/11299538?v=4&u=ed9ce1c9151d552d985bdf5546772e14ef7ab617&s=144
[konstantin_web]: https://github.com/comeanother/

[sergey_img]: https://avatars1.githubusercontent.com/u/1134449?v=4&u=ed9ce1c9151d552d985bdf5546772e14ef7ab617&s=144
[sergey_web]: https://github.com/s2504s/

[valeriy_img]: https://avatars1.githubusercontent.com/u/10601658?v=4&u=ed9ce1c9151d552d985bdf5546772e14ef7ab617&s=144
[valeriy_web]: https://github.com/drama17/

[vladimir_img]: https://avatars1.githubusercontent.com/u/26582191?v=4&u=ed9ce1c9151d552d985bdf5546772e14ef7ab617&s=144
[vladimir_web]: https://github.com/SweetOps/
