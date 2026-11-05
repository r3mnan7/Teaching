---
title: "Hosting platform for multiple users"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part4/multi-juicer.html
tags: [juiceshop, owasp, companion-guide]
---

# Hosting platform for multiple users

← Previous: [[02 Customization|Customization]]
Next: [[04 Hosting a CTF event|Hosting a CTF event]] →

![[part4-multi-juicer-multi-juicer_cover.svg]]

The recommended solution to host and manage individual Juice Shop instances for multiple users (e.g. for CTF events or workshops) is [MultiJuicer](https://github.com/iteratec/multi-juicer/). MultiJuicer is a Kubernetes based system to start up the required Juice Shop instances on demand. It will also clean up unused instances after a configured period of inactivity.

MultiJuicer v10.4.0 comes with a custom-built load balancer. It allows every participant (or a CTF team) to use the same URL, but their traffic will always be sent to their own personal (or team-specific) instance.

![[part4-multi-juicer-multi-juicer_high-level-architecture.png]]

Registration at MultiJuicer is very straightforward for the users/teams. Messing with other instances is prevented by assigning a passcode to each of them which should only be shared among team members - or not at all for individuals.

![[part4-multi-juicer-multi-juicer_register.png]]

MultiJuicer comes with a Score Board of its own, which allows teams to compare their progress through the Juice Shop challenges. Teams can even drill down into their own team-specific or challenge-specific details. This Score Board is often sufficient for most workshops and trainings. If you require a more traditional Jeopardy-style CTF experience with flag codes and multiple frameworks, please refer to [[04 Hosting a CTF event|Hosting a CTF event]].

![[part4-multi-juicer-multi-juicer_scoreboard.png]]

![[part4-multi-juicer-multi-juicer_scoreboard_team.png]]

![[part4-multi-juicer-multi-juicer_scoreboard_challenge.png]]

For administrators, MultiJuicer provides a convenient overview of all active teams along with their challenge progress, current [[02 Cheat detection|cheat score]], and the possibility to reset or delete their Juice Shop instance.

![[part4-multi-juicer-multi-juicer_admin_view.png]]

Another helpful feature for trainers and CTF-organizers is the [optional dashboard which automatically consumes and displays metrics](https://github.com/juice-shop/multi-juicer/blob/main/guides/monitoring-setup/monitoring.md) from each of its Juice Shop instances. It shows individual challenge progress along with other functional and technical stats and can be very helpful in troubleshooting as well.

![[part4-multi-juicer-multi-juicer_dashboard.png]] ![[part4-multi-juicer-multi-juicer_dashboard1.png]]

The MultiJuicer repository offers guidance on how to set up the system on different cloud provider platforms:

- [AWS](https://github.com/juice-shop/multi-juicer/tree/main/guides/aws)

- [Azure](https://github.com/juice-shop/multi-juicer/tree/main/guides/azure)

- [Digital Ocean](https://github.com/juice-shop/multi-juicer/tree/main/guides/digital-ocean)

- [Kubernetes](https://github.com/juice-shop/multi-juicer/tree/main/guides/k8s)

- [Openshift](https://github.com/juice-shop/multi-juicer/tree/main/guides/openshift)

- [[05 Setting up the Juice Shop Lego Tower|Raspberry Pi (Juice Shop Lego Tower)]]

[[02 Customization|Customization]] [[04 Hosting a CTF event|Hosting a CTF event]]

