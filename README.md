# Kiobi

Abstraction for Kibana REST API `saved_objects/_find` for retrieving objects and `saved_objects/_bulk_create` for storing objects.

Possible use cases:
- Migrate Kibana objects between two Kibana instances.
- Backup Kibana objects to a file.
- Restore Kibana objects from file.

## Requirements

None

## Role Variables

`source_host` - source kibana host
`source_parameters` - [url query parameters](https://www.elastic.co/guide/en/kibana/current/saved-objects-api-find.html), default is `type=visualization&type=search&type=index-pattern&type=dashboard&per_page=100&page=1`
`source_objects` - list of additional Kibana objects
`source_file` - path to source file
`destination_host` - destination kibana hostname
`destination_parameters` - [url query parameters](https://www.elastic.co/guide/en/kibana/current/saved-objects-api-bulk-create.html), default is `overwrite=false`
`destination_file` - path to destination file

## Dependencies

None

## Example Playbooks

- Playbook for migrating Kibana objects:
```
- hosts: localhost
  roles:
    - role: kiobi
      vars:
        source_host: "localhost:5601"
        source_parameters: "type=search&type=index-pattern&per_page=10&page=1"
        destination_host: "localhost:5602"
        destination_parameters: "overwrite=true"
```

- Playbook for restoring Kibana objects:
```
- hosts: localhost
  roles:
    - role: kiobi
      vars:
        source_file: "/tmp/saved_objects.json"
        source_objects:
          - id: "43s5rcv7b8yb72g83b8fy"
            type: "index-pattern"
            attributes:
              title: "test-*"
        destination_host: "localhost:5601"
```

- Playbook for backuping Kibana objects:
```
- hosts: localhost
  roles:
    - role: kiobi
      vars:
        source_host: "localhost:5601"
        destination_file: "/tmp/test_output.json"
```

## License

MIT

## Author Information

Tomas Bellus
