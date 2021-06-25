SRE Notes

## Security and Reliability

https://static.googleusercontent.com/media/sre.google/en//static/pdf/building_secure_and_reliable_systems.pdf

## Design Considerations

- General - In designing for reliability and security, you must consider different risks. The primary reliability risks are nonmalicious in nature — for example, a bad software update or a physical device failure. Security risks, however, come from adversaries who are actively trying to exploit system vulnerabilities. When designing for reliability, you assume that some things will go wrong at some point. When designing for security, you must assume that an adversary could be trying to make things go wrong at any point.

- Redundany - In designing for reliability, you often need to add redundancy to systems. While redundancy increases reliability, it also increases the attack surface. An adversary need only find a vulnerability in one path to be successful.

- Incident Management - Reliability incidents benefit from having responders with multiple perspectives who can help find and mitigate the root cause quickly. By contrast, you’ll often want to handle security incidents with the smallest number of people who can fix the problem effectively, so the adversary isn’t tipped off to the recovery effort. In the security case, you’ll share information on a need-to-know basis. Similarly, voluminous system logs may inform the response to an incident and reduce your time to recovery, but—depending on what is logged— those logs may be a valuable target for an attacker.
- 

