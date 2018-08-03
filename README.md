# haproxy-cluster-demo

Creates a two node cluster with a haproxy sitting in front.

## Requirements

* [Vagrant](https://www.vagrantup.com/)

## Usage

1. Clone the repo and vagrant up:

    ```
    git clone git@github.com:adrianjuhl/haproxy-cluster-demo.git
    cd haproxy-cluster-demo
    vagrant up
    ```

2. Browse to:

  * http://192.168.1.0/probe.html - the probe file of node 1
  * http://192.168.2.0/probe.html - the probe file of node 2
  * http://192.168.3.0/probe.html - the probe file of one of the nodes in the pool, returned via the haproxy

3. Browse to http://192.168.3.0/haproxy?stats, login with user/password, to view the haproxy stats page.

4. Take node 1 out of the pool and observe the result:

    ```
    vagrant ssh node1
    sudo mv /var/www/html/probe.html /var/www/html/probe.html.probe_off
    ```

  Browse once again to http://192.168.3.0/probe.html and observe that only the probe file of node 2 is returned.

## Dependencies

None

## See Also

None

## License

MIT

## Source Code

[https://github.com/adrianjuhl/haproxy-cluster-demo](https://github.com/adrianjuhl/haproxy-cluster-demo)

## Author Information

[Adrian Juhl](http://github.com/adrianjuhl)
