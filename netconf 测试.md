# netconf 测试

华为 ce6800 netconf

交换机netconf 配置

ssh user arpconf
ssh user arpconf authentication-type password
ssh user arpconf service-type snetconf

aaa
  local-user arpconf password cipher Arpnet@123
  local-user arpconf service-type ssh
  local-user arpconf level 3

snetconf server enable



netconf 配置 
- 创建vlan
```
<config xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <vlan xmlns="http://www.huawei.com/netconf/vrp/huawei-vlan">
        <vlans>
          <vlan xc:operation="create">
            <vlanId>20</vlanId>
            <vlanName>VLAN20</vlanName>
            <vlanType>common</vlanType>
          </vlan>
          <vlan xc:operation="create">
            <vlanId>30</vlanId>
            <vlanName>VLAN30</vlanName>
            <vlanType>common</vlanType>
          </vlan>
          <vlan xc:operation="create">
            <vlanId>40</vlanId>
            <vlanName>VLAN40</vlanName>
            <vlanType>common</vlanType>
          </vlan>
        </vlans>
      </vlan>
    </config>

修改vlan
 <config xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <vlan xmlns="http://www.huawei.com/netconf/vrp/huawei-vlan">
        <vlans>
          <vlan xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="replace">
            <vlanId>20</vlanId>
            <vlanName>VLAN20</vlanName>
            <vlanType>common</vlanType>
          </vlan>
          <vlan xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="replace">
            <vlanId>30</vlanId>
            <vlanName>VLAN-30</vlanName>
            <vlanType>common</vlanType>
          </vlan>
          <vlan xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="replace">
            <vlanId>40</vlanId>
            <vlanName>VLAN-40</vlanName>
            <vlanType>common</vlanType>
          </vlan>
        </vlans>
      </vlan>
    </config>
```

- 端口vlan 
```
<config xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <ethernet xmlns="http://www.huawei.com/netconf/vrp/huawei-ethernet">
        <ethernetIfs>
          <ethernetIf>
            <ifName>GE1/0/2</ifName>
            <l2Attribute xc:operation="create">
              <linkType>access</linkType>
              <pvid>10</pvid>
            </l2Attribute>
          </ethernetIf>
          <ethernetIf>
            <ifName>GE1/0/3</ifName>
            <l2Attribute xc:operation="create">
              <linkType>access</linkType>
              <pvid>20</pvid>
            </l2Attribute>
          </ethernetIf>
        </ethernetIfs>
      </ethernet>
    </config>
```
| 配置trunk
```
<config xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <ethernet xmlns="http://www.huawei.com/netconf/vrp/huawei-ethernet">
        <ethernetIfs>
          <ethernetIf>
            <ifName>GE1/0/2</ifName>
            <l2Attribute xc:operation="create">
              <linkType>trunk</linkType>
              <trunkVlans>10,20</trunkVlans>
            </l2Attribute>
          </ethernetIf>
        </ethernetIfs>
      </ethernet>
    </config>
```

配置IP
```
<config xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
 <ethernet xmlns="http://www.huawei.com/netconf/vrp/huawei-ethernet">
        <ethernetIfs>
          <ethernetIf xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="replace">
            <ifName>GE1/0/5</ifName>
            <l2Enable>disable</l2Enable>
          </ethernetIf>
        </ethernetIfs>
      </ethernet>
      <ifm xmlns="http://www.huawei.com/netconf/vrp/huawei-ifm">
        <interfaces>
          <interface>
            <ifName>GE1/0/5</ifName>
            <ifDescr>55555555555555555555</ifDescr>
            <ifAdminStatus>up</ifAdminStatus>
            <ipv4Config>
              <am4CfgAddrs>
                <am4CfgAddr xc:operation="create">
                  <ifIpAddr>192.168.5.1</ifIpAddr>
                  <subnetMask>255.255.255.0</subnetMask>
                  <addrType>main</addrType>
                </am4CfgAddr>
              </am4CfgAddrs>
            </ipv4Config>
          </interface>
        </interfaces>
      </ifm>
    </config>

修改ip 地址  要先删除，再添加
<am4CfgAddrs>
  <am4CfgAddr xc:operation="delete">
    <ifIpAddr>192.168.2.1</ifIpAddr>
    <addrType>main</addrType>
    <subnetMask>255.255.255.0</subnetMask>
  </am4CfgAddr>
  <am4CfgAddr xc:operation="create">
    <ifIpAddr>192.168.4.1</ifIpAddr>
    <subnetMask>255.255.255.0</subnetMask>
    <addrType>main</addrType>
  </am4CfgAddr>
</am4CfgAddrs>
```
