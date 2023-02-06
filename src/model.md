# `build_generator`
```python
o_mask, fea_m = self.build_mask_prediction_net(i_s)
o_b, _ = self.background_inpainting_net(i_s, o_mask, fea_m)
```
- Input:
    - `i_s`: Text image
- Output:
    - `o_b`: Text-removed image
    - `o_mask`: Text stroke mask

- Stroke Mask Prediction Module (SMPM):
    - `_conv_bn_relu`, `build_encoder_net`, `_res_block`, `build_res_net`, `build_decoder_net`, `build_mask_prediction_net`
- Background Inpainting Module (BIPM):
    - `ContextBlock2d`, `build_attention_block`, `PartialConv2d`, `UpsampleConcat`, `PConvActiv`, `PConvUNet`