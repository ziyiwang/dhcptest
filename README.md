## dhcptest with relayAgent Information
an extension to Cybershadow's version, to support option 82 sub options example as follows:

./dhcptest --query --option "82=1.3.12.32.15"

1 -------> suboption 1
3 -------> suboption length \n
12.32.15 -----> circuit id

will refine syntax later one, current version is to enable testing

sample generated packet capture from Cisco PNR:
Generetaed circuit ID

R1329: ----- RECEIVED -- R1329 -----
R1329: -> port = 68 received from = 10.183.16.155
R1329: -> packet length = 251
R1329: -> op = 1 request
R1329: -> htype = 1 ethernet hlen = 6
R1329: -> hops = 0
R1329: -> xid = 0x36e902a5 secs = 0 flags = 0x8000 broadcast
R1329: -> ciaddr = 0.0.0.0
R1329: -> yiaddr = 0.0.0.0
R1329: -> siaddr = 0.0.0.0
R1329: -> giaddr = 10.183.16.155
R1329: -> chaddr = 7b:0b:78:74:7f:81
R1329: -> dhcp-message-type = DHCPDISCOVER
R1329: -> relay-agent-info = 01:03:0c:20:0f
R1329: -> circuit-id 1 0c:20:0f
R1329: -> end
R1329: -> sname = ""
R1329: -> file = ""
R1329: ----- END OF RECEIVED -- R1329 -----




## dhcptest

This is a DHCP test tool. It can send DHCP discover packets, and listen for DHCP replies.

The tool is cross-platform, although you will need to compile it yourself for non-Windows platforms.

The tool is written in the [D Programming Language](http://dlang.org/).

## Download

You can download a compiled Windows executable from my website, [here](http://files.thecybershadow.net/dhcptest/).

## Building

With [DMD](http://dlang.org/download.html#dmd) (or another D compiler) installed, run:

```
$ dmd dhcptest.d
```

## Usage

By default, dhcptest starts in interactive mode.
It will listen for DHCP replies, and allow sending DHCP discover packets using the "d" command.
Type `help` in interactive mode for more information.

If you do not receive any replies, try using the `--bind` option (or `--iface` on Linux) to bind to a specific local interface.

The program can also run in automatic mode if the `--query` switch is specified on the command line.

An example command line to automatically send a discover packet and explicitly request option 43,
wait for a reply, then print just that option:

    dhcptest --quiet --query --request 43 --print-only 43

Options can also be specified by name:

    dhcptest --quiet --query \
         --request    "Vendor Specific Information" \
         --print-only "Vendor Specific Information"

Query mode will report the first reply recieved. To automatically send a discover packet and wait for 
all replies before the timeout, use `--wait`. For additional resilience against dropped packets on busy 
networks, consider using the `--retry` and `--timeout` switches:

    dhcptest --quiet --query --wait --retry 5 --timeout 10

You can spoof the Vendor Class Identifier, or send additional DHCP options with the request packet,
using the `--option` switch:

    dhcptest --query --option "60=Initech Groupware"

Run `dhcptest --help` for further details and additional command-line parameters.

For a list and description of DHCP options, see [RFC 2132](http://tools.ietf.org/html/rfc2132).

## License

`dhcptest` is available under the [Boost Software License 1.0](http://www.boost.org/LICENSE_1_0.txt).

## Changelog

### dhcptest v0.7 (2017-08-03)

 * Refactor and improve option value parsing
 * Allow specifying all supported format types in both --option and
   --print-only switches
 * Allow specifying DHCP option types by name as well as by number
 * Allow overriding the request type option. E.g., you can now send
   'request' (instead of 'discover') packets using:

        --option "DHCP Message Type=request"

 * Add formatting support for options 42 (Network Time Protocol
   Servers Option) and 82 (Relay Agent Information)
 * Change how timeouts are handled:
   * Always default to some finite timeout (not just when `--tries`
     and `--wait` are absent), but still allow waiting indefinitely if
     0 is specified.
   * Increase default timeout from 10 to 60 seconds.

### dhcptest v0.6 (2017-08-02)

 * Add `--secs` switch
 * Contributed by [Darren White](https://github.com/DarrenWhite99):
     * Add `--wait` switch
     * The `--print-only` switch now understands output formatting:
       `--print-only "N[hex]"` will output the value as a zero padded hexadecimal string of bytes.
       `--print-only "N[ip]"` will output the value as an IP address.
 * Don't print stack trace on errors

### dhcptest v0.5 (2014-11-26)

 * The `--option` switch now understands hexadecimal or IPv4-dotted-quad formatting:  
   `--option "N[hex]=XX XX XX ..."` or `--option "N[IP]=XXX.XXX.XXX.XXX"`

### dhcptest v0.4 (2014-07-21)

 * Add switches: `--retry`, `--timeout`, `--option`

### dhcptest v0.3 (2014-04-05)

 * Add switches: `--mac`, `--quiet`, `--query`, `--request`, `--print-only`
 * Print program messages to standard error

### dhcptest v0.2 (2014-03-25)

 * License under Boost Software License 1.0
 * Add documentation
 * Add `--help` switch
 * Add `--bind` switch to specify the interface to bind on
 * Print time values in human-readable form
 * Heuristically detect and print ASCII strings in unknown options
 * Add option names from RFC 2132
 * Add `help` and `quit` commands
 * Add MAC address option to `discover` command

### dhcptest v0.1 (2013-01-10)

 * Initial release
