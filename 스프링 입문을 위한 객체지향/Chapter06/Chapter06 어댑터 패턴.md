![image-20250219005034512](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250219005034512.png)

## 어댑터 패턴이란?

- 서로 호환되지 않는 인터페이스를 가진 클래스들이 함께 동작할 수 있도록 변환해주는 디자인 패턴
  - 서로 다른 인터페이스를 가진 클래스들 간의 호환성을 제공해준다.
  - **어댑터는 한 객체의 인터페이스를 구현하고 다른 객체는 래핑**한다.

### 사용 목적

- 인터페이스 호환성 제공

  - 클라이언트가 기대하는 인터페이스와 실제 기능을 수행하는 클래스(또는 시스템)의 인터페이스가 다를 때, 이를 중간에서 변환해주는 역할을 합니다.

- 기존 코드의 재사용

  - 이미 구현되어 있는 클래스를 수정하지 않고도 새로운 시스템에 통합할 수 있습니다.

    

Adapter 패턴은 두 가지 방법으로 나뉩니다

그 중 자바에서 사용하는 것은 객체 어뎁터 패턴입니다.

- Object Adapter pattern
  - 합성된 멤버에게 위임을 이용한 어뎁터 패턴
  - 자기가 해야 할 일을 클래스 맴버 객체의 메소드에게 다시 시킴으로써 목적을 달성하는 것을 위임이라고 합니다.
  - 합성을 활용했기 때문에 런타임 중에 Adaptee(Service)가 결정되어 유연하다
    <br>
- **Target (목표 인터페이스)**:
  - **클라이언트가 사용하고자 하는 인터페이스를 정의합니다.**
- **Adaptee (기존 클래스/인터페이스)**:
  - **기존에 존재하는, 클라이언트가 직접 사용하기에는 인터페이스가 다른 클래스**입니다.
- **Adapter (어댑터 클래스)**:
  - **Target 인터페이스를 구현하며, 내부적으로 Adaptee 객체를 호출해 클라이언트의 요청을 적절하게 변환하여 전달합니다.**
- **Client(클라이언트)** :
  - 기존 시스템을 어댑터를 통해 이용하려는 쪽.
  - Client Interface를 통하여 Service를 이용할 수 있게 된다.
    
<br>

```java
// Target 인터페이스: 클라이언트가 사용하는 인터페이스
interface MediaPlayer {
    void play(String audioType, String fileName);
}

// Adaptee 인터페이스: 확장된 기능을 가진 플레이어 인터페이스
interface AdvancedMediaPlayer {
    void playVlc(String fileName);
    void playMp4(String fileName);
}

// Adaptee의 구현체: VLC 파일 재생 기능을 제공
class VlcPlayer implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        System.out.println("Playing VLC file. Name: " + fileName);
    }

    @Override
    public void playMp4(String fileName) {
        // VLC 플레이어는 MP4 파일 재생 기능이 없음
    }
}

// Adaptee의 구현체: MP4 파일 재생 기능을 제공
class Mp4Player implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        // MP4 플레이어는 VLC 파일 재생 기능이 없음
    }

    @Override
    public void playMp4(String fileName) {
        System.out.println("Playing MP4 file. Name: " + fileName);
    }
}

// Adapter 클래스: AdvancedMediaPlayer를 MediaPlayer 인터페이스에 맞게 변환
class MediaAdapter implements MediaPlayer {
    AdvancedMediaPlayer advancedMusicPlayer;

		// 내부적으로 Adaptee 객체를 호출하고 이를 적절하게 변환한다.
    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedMusicPlayer = new VlcPlayer();
        } else if (audioType.equalsIgnoreCase("mp4")) {
            advancedMusicPlayer = new Mp4Player();
        }
    }

    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedMusicPlayer.playVlc(fileName);
        } else if (audioType.equalsIgnoreCase("mp4")) {
            advancedMusicPlayer.playMp4(fileName);
        }
    }
}

// 클라이언트 클래스: AudioPlayer는 기본적으로 mp3 파일을 재생하며, 
// 다른 포맷의 파일은 어댑터를 통해 재생
class AudioPlayer implements MediaPlayer {
    MediaAdapter mediaAdapter;

    @Override
    public void play(String audioType, String fileName) {
        // mp3는 기본 지원
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing MP3 file. Name: " + fileName);
        }
        // mp3가 아니면 어댑터 사용
        else if (audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")) {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        }
        else {
            System.out.println("Invalid media. " + audioType + " format not supported");
        }
    }
}

// 데모 클래스
public class AdapterPatternDemo {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();

        audioPlayer.play("mp3", "beyond_the_horizon.mp3");
        audioPlayer.play("mp4", "alone.mp4");
        audioPlayer.play("vlc", "far_far_away.vlc");
        audioPlayer.play("avi", "mind_me.avi");
    }
}
```

- 장점
  - 기존 코드의 재사용성을 높입니다.
  - 클라이언트 코드를 수정하지 않고도 새로운 기능을 추가할 수 있습니다.
  - 다양한 인터페이스를 일관된 방식으로 사용할 수 있습니다.
- 단점
  - 어댑터 계층이 추가되면 코드가 복잡해질 수 있습니다.
  - 인터페이스 변환 과정에서 성능 저하가 발생할 가능성이 있습니다.
