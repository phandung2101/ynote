g# Intent
**Bridge** is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.
# Problem
```java
// TV logic
class TV {
    private boolean on = false;
    private int volume = 50;
    private int channel = 1;

    public void turnOn() { on = true; }
    public void turnOff() { on = false; }
    public boolean isOn() { return on; }

    public int getVolume() { return volume; }
    public void setVolume(int volume) { this.volume = volume; }

    public int getChannel() { return channel; }
    public void setChannel(int channel) { this.channel = channel; }
}

// Remote only works with TV
class TVRemote {
    private final TV tv;

    public TVRemote(TV tv) {
        this.tv = tv;
    }

    public void togglePower() {
        if (tv.isOn()) {
            tv.turnOff();
        } else {
            tv.turnOn();
        }
    }

    public void volumeUp() {
        tv.setVolume(tv.getVolume() + 10);
    }

    public void channelDown() {
        tv.setChannel(tv.getChannel() - 1);
    }

    // ...
}

```
# Solution
```java
  
interface Device {  
    boolean isEnable();  
  
    void enable();  
  
    void disable();  
  
    int getVolume();  
  
    void setVolume(int percent);  
  
    int getChanel();  
  
    void setChanel(int chanel);  
}  
  
static class RemoteControl {  
    protected final Device device;  
  
    RemoteControl(final Device device) {  
        this.device = device;  
    }  
  
    void togglePower() {  
        if (device.isEnable()) {  
            device.disable();  
        } else {  
            device.enable();  
        }  
    }  
  
    void volumeDown() {  
        device.setVolume(device.getVolume() - 10);  
    }  
  
    void volumeUp() {  
        device.setVolume(device.getVolume() + 10);  
    }  
  
    void chanelDown() {  
        device.setChanel(device.getChanel() - 1);  
    }  
  
    void chanelUp() {  
        device.setChanel(device.getChanel() + 1);  
    }  
}  
  
static class AdvancedRemoteControl extends RemoteControl {  
  
    AdvancedRemoteControl(final Device device) {  
        super(device);  
    }  
  
    void mute() {  
        device.setVolume(0);  
    }  
}  
  
static class TV implements Device { ... }  
static class Radio implements Device { ... }  

void clientCode () {  
    var tv = new TV();  
    var remote = new RemoteControl(tv);  
    remote.togglePower();  
  
    var radio = new Radio();  
    remote = new AdvancedRemoteControl(radio);  
}
```

We can easily extend our remote control with new features without depend what device we can dealing with 