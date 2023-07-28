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
- 配置trunk
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
          <ethernetIf>
            <ifName>GE1/0/3</ifName>
            <l2Attribute xc:operation="create">
              <linkType>trunk</linkType>
              <trunkVlans>10,20</trunkVlans>
            </l2Attribute>
          </ethernetIf>
        </ethernetIfs>
      </ethernet>
    </config>
```
