COROPITを使用する方は以下のようにコードを編集してください。

mona2_r.overlay

修正前
```
  trackball_central: trackball_central@0 {
        status = "okay";
        compatible = "pixart,pmw3610";  //トラボセンサ用のドライバとバインド
        reg = <0>;
        spi-max-frequency = <2000000>;
        irq-gpios = <&gpio0 2 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>; //P0.02を指定(MOTION)
        cpi = <600>;
        //swap-xy;
        //invert-x; //COROPIT版ではコメントアウトを外す
        //invert-y; //COROPIT版ではコメントアウトを外す
        evt-type = <INPUT_EV_REL>;
        x-input-code = <INPUT_REL_X>;
        y-input-code = <INPUT_REL_Y>;
    };
};

```
**修正後**
```
  trackball_central: trackball_central@0 {
        status = "okay";
        compatible = "pixart,pmw3610";  //トラボセンサ用のドライバとバインド
        reg = <0>;
        spi-max-frequency = <2000000>;
        irq-gpios = <&gpio0 2 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>; //P0.02を指定(MOTION)
        cpi = <600>;
        //swap-xy;
        invert-x; //COROPIT版ではコメントアウトを外す
        invert-y; //COROPIT版ではコメントアウトを外す
        evt-type = <INPUT_EV_REL>;
        x-input-code = <INPUT_REL_X>;
        y-input-code = <INPUT_REL_Y>;
    };
};

```

## AMLガード設定メモ
- マウスレイヤー（MOUSE=5）の自動切替に、キー入力中は発動しないガードを追加しました。`boards/shields/mona2/mona2.dtsi` 内の `aml_temp_layer` で `require-prior-idle-ms = <400>;` を指定しています。
- これにより、キー入力後 400ms 以内にトラックボールへ触れてもマウスレイヤーに上がりません。必要に応じて値を調整してください。
- `excluded-positions` はマウスボタンを割り当てたキー位置のみに設定しています（位置 11,12,13,17,18,19 = MB1/2/3）。そのためクリックではレイヤーが落ちませんが、他のキー（文字キーなど）を押すとレイヤーが即解除されます。
