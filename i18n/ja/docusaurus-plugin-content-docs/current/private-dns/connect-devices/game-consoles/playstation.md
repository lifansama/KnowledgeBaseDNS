---
title: PlayStation PS4/PS5
sidebar_position: 4
---

ゲーム機は暗号化されたDNSをサポートしていませんが、リンクされたIPアドレスを介してパブリックAdGuard DNSまたはプライベートAdGuard DNSを設定できます。

お使いのルーターが暗号化DNSサーバーの使用をサポートしている可能性が高いため、ルーターにプライベートAdGuard DNSを設定し、ゲーム機をルーター（Wi-Fi）に接続するという方法もあります。そうすれば、ゲーム機は暗号化AdGuard DNSに接続されます。

[ルーターでの設定方法はこちら](/private-dns/connect-devices/routers/routers.md)

## AdGuard DNSに接続する方法

パブリックAdGuard DNSサーバーを使用するようにゲーム機を設定するか、リンクされたIPを介して設定します:

### PlayStation 4 の場合

1. ゲーム機(PS4)を起動して、アカウントにサインインしてください。
2. PS4のホーム画面から、最上段にある歯車アイコン（⚙）を選択します。
3. _設定_ → _ネットワーク_ → _設定_ に移動します。
4. 「インターネット接続を設定する」を選択します。
5. _LANケーブルを使用する_→_簡単_ を選択します。
6. 「_手動_」を選択し、「_IPアドレス設定_」で「_自動_」を選択します。
7. 「DHCPホスト名」では、「指定しない」を選択します。
8. 「_DNS 設定_」で、「_手動_」を選択します。
9. 「_DNS サーバー_」欄に、次のいずれかの DNS サーバーアドレスを入力します:
    - `94.140.14.49`
    - `94.140.14.59`
10. 「_次へ_」を選択して続行します。
11. 「_MTU 設定_」画面で、「_自動_」を選択します。
12. 「_プロキシサーバー_」画面で、「_使用しない_」を選択します。
13. 「_インターネット接続のテスト_」（接続診断）を選択して、新しいDNS設定をテストします。
14. テスト（接続診断）が完了し、「_インターネット接続：成功_」と表示されたら、設定を保存します。

### PlayStation 5 の場合

1. ゲーム機(PS5)を起動して、アカウントにサインインしてください。
2. PS4のホーム画面から、最上段にある歯車アイコン（⚙）を選択します。
3. _設定_ → _ネットワーク_ → _設定_ に移動します。
4. 「インターネット接続を設定する」を選択します。
5. 「有線LANを設定する」 → 「接続」を選択します。
6. 「_手動_」を選択し、「_IPアドレス設定_」で「_自動_」を選択します。
7. 「DHCPホスト名」では、「指定しない」を選択します。
8. 「_DNS 設定_」で、「_手動_」を選択します。
9. 「_DNS サーバー_」欄に、次のいずれかの DNS サーバーアドレスを入力します:
    - `94.140.14.49`
    - `94.140.14.59`
10. 「_次へ_」を選択して続行します。
11. 「_MTU 設定_」画面で、「_自動_」を選択します。
12. 「_プロキシサーバー_」画面で、「_使用しない_」を選択します。
13. 「_インターネット接続のテスト_」（接続診断）を選択して、新しいDNS設定をテストします。
14. テスト（接続診断）が完了し、「_インターネット接続：成功_」と表示されたら、設定を保存します。

リンクされたIP（チームプランをご利用の場合は専用IP）を使用するのがおすすめです:

- [専用IP](/private-dns/connect-devices/other-options/dedicated-ip.md)
- [リンクされたIP](/private-dns/connect-devices/other-options/linked-ip.md)
