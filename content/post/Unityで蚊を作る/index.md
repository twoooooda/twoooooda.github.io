+++
author = "twoooooda"
title = "Unityで蚊っぽい動きをするやつを作る"
date = "2021-04-02"
description = "蚊特有の、あのウザい挙動をするオブジェクトを作ってみました。"

tags = [
    "知見",
    "備忘録",
    "C#",
    "Unity"
]

categories = [
    "Unity"
]

[[images]]
src = "img/2021/ka.png"
+++

# はじめに
　 少し前にOculus Quest2を購入し、しばらくハンドトラッキングで遊んでいたんですが、急に『手で蚊を潰すVRゲーム』を作りたくなったのでとりあえず蚊っぽい挙動をするナニカを作りました。 
<br> 
<br>

# 1.スクリプト
　やっていることはごく簡単で、数フレームに一回ランダムでRotationの値を変更し、Z方向に常に力を加え続けているだけです。

```cs:Move_mosquite.cs
using UnityEngine;

public class Move_mosquite : MonoBehaviour
{
    Rigidbody rb;
    public float freq, speed, power;

    void Start()
    {
        rb = this.GetComponent<Rigidbody>();
    }

    void Update()
    {
        rb.velocity = rb.velocity.normalized * speed; //変数speedで速度の設定
        rb.AddForce(this.transform.forward * power * Time.deltaTime, ForceMode.Force); //Z方向(forward)に力を加え続ける

        if (Random.value < freq) //Rotationを変更する頻度を設定。毎フレームだと頻繁過ぎるので。
        {
            transform.eulerAngles = new Vector3(Random.Range(0, 360), Random.Range(0, 360), Random.Range(0, 360));
        }
    }
}
``` 
<br>

このままでは無限にどっかいってしまうので、適当にコライダーをつけるとか、壁に近づくと力を受けるとかにするといいかもしれません。<br>
<br>
なお、当方めちゃくちゃ初心者なのでもっと効率的なやり方や、もっとリアルになるやり方があるかもしれませんのであしからず....

# 完成例

{{< youtube SFOzY6yHWQc >}} 

<br>
<br>
<br>

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=twoooooda-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B08GCM963G&linkId=b4e6fb07c9c67cab61639650f1e8bb38"></iframe>