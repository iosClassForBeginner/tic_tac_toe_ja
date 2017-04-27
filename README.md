# 第18回: １時間でiPhoneアプリを作ろう
## マルバツゲームアプリ

<img src=https://raw.githubusercontent.com/iosClassForBeginner/tic_tac_toe_ja/master/screen.PNG width=400px>
  
  当アカウントへ訪れていただき、誠にありがとうございます。第18回アプリ教室では、マルバツゲームアプリを作ります。自分のペースで勉強したい、勉強前に予習したい、内容を復習したい場合、ご利用ください。
  
## アプリ教室に興味ある方、歓迎します。  
  Meetup  
  http://www.meetup.com/ios-dev-in-namba/
  
## 別途アプリ教室(有料)も開いております  
  http://learning-ios-dev.esy.es/  

## 問い合わせ
  株式会社ジーライブ
  http://geelive-inc.com  

## 開発環境
  XCode 8.1 / Swift 3

## アプリ作成手順

#### 0, プロジェクト作成
> 0-1. XCodeを起動する。

> 0-2. "Create a new XCode project"を選択します。

> 0-3. "Single View Application"を選択し、"Next"をタップします。

> 0-4. "Product name" を入力し、"Next"をタップします。

> 0-5. プロジェクトを保存する場所を選択し、"Create"をタップします。

  <div style="text-align:center"><img src="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid1.gif" /></div>

#### 1, 写真素材をダウンロードし、Assets.xcassets にドラッグ&ドロップで追加する。
  <a href="https://raw.githubusercontent.com/iosClassForBeginner/tic_tac_toe_ja/master/resources.zip">resources</a>
  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid2.gif" /></div>

#### 2, Design app
> 2-1. 
>> 1. main.storyboardを選択し、UI部品からDrop "UIImageView"を配置します。(ドラッグ&ドロップ)
>> 2. "UIImageView"を適切な大きさにします。
>> 3. "UIImageView"を適切な場所におきます。
>> 4. "UIImageView"の画像を選択します。

  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid3.gif" /></div>

> 2-2. ボードの上にボタンをのせる。
>> 1. main.storyboardを選択し、UI部品からDrop "UIButton"を配置します。(ドラッグ&ドロップ)
>> 2. "UIButton"を適切な大きさにします。
>> 3. "UIButton"の画像を選択します。
>> 4. コピーでボタンを9個作ります
>> 5. "UIButton"を適切な場所におきます。

  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid4.gif" /></div>
  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid5.gif" /></div>

> 2-3. 0から8まで(全部で9個) tagをそれぞれのボタンにタグを設定する。

  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid6.gif" /></div>

> 2-4. StoryboardのUIButtonを、ViewController.swiftに紐づけます（control押しながらドラッグ）
>> 1. "action"を作成し、名前を付けます（「buttonPressed」など）。
>> 2. 残りの8つのボタンについては、同じ@IBActionにドラッグするだけです

  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid7.gif" /></div>

> 2-5. 勝者を表示するUILabelと、リスタートするためのUIButtonを作成します。

  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid8.gif" /></div>
  
> 2-6. Storyboardの勝者を表示するUILabelとリスタートするためのUIButtonを、ViewController.swiftに紐づけます（control押しながらドラッグ）
>> 1. 勝者を表示するUILabelはOutlet接続
>> 2. リスタートするためのUIButtonはOutlet接続とaction接続の両方

  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/tic_tac_toe_ja/blob/master/videos/vid9.gif" /></div>

#### 3, 以下コードブロックを記入
- 以下コードブロックを記入
  ★  コピペでも大丈夫ですが、自分で書くと理解を深めることができます。

```Swift  
//  Created by Sam on 2017-04-17.
//  Copyright © 2017 Sam. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var winner: UILabel!
    @IBOutlet weak var playButton: UIButton!
    
    var gameIsActive = true // A Boolean value to keep track when the game ends.
    
    // Players by their number reference 
    // 0 is unplayed button
    // 1 is Cross
    // 2 is Noughts
    
    var activePlayer = 1 // We start with Cross || Challenge: Try starting with the winner i.e. store the value of the winner
    var gameState = [0 ,0 ,0 ,
                     0 ,0 ,0 ,
                     0 ,0 ,0 ] // From left to right and top to bottom these numbers represent the status of each button
    
    /*
        Tag representation of our button matrix
     
                | 0 | 1 | 2 |
                | 3 | 4 | 5 |
                | 6 | 7 | 8 |
     
        Possible combinations are:
 */
    let winningCombinations = [[0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
                               [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
                               [0, 4, 8], [2, 4, 6]] // Diagonals
    
    
    @IBAction func buttonPressed(_ sender: UIButton) {
        if gameState[sender.tag] == 0 { // We get the tag of the button and if its state is 0 it means it hasnt been altered.
            gameState[sender.tag] = activePlayer // We change the state of that position to the players number
            
            
            // Now we need to change the buttons image to the right representation of the player and change the active player to the other player
            
            if activePlayer == 1 {
                sender.setImage(UIImage(named: "x") , for: .normal)
                activePlayer = 2
            } else {
                sender.setImage(UIImage(named: "o") , for: .normal)
                activePlayer = 1
            }
            
            
            
            // It's possible at this point someone has won the game, so we need to check if the states of our buttons match any of the winning combinations. We need to use a for loop to search through each winning combination and if we found a match, game is over
            
            for combination in winningCombinations {
                // This will go through all the possible combinations in the winningCombinations array. Combination could be [2, 5, 8] as an example here
                
                if (gameState[combination[0]] != 0) && (gameState[combination[0]] == gameState[combination[1]]) && (gameState[combination[1]] == gameState[combination[2]]) {
                    // Here we want to make sure the state number is not 0 and all 3 positions are the same state. For example, 1 for the cross.
                    
                    gameIsActive = false // Game ends because a winning condition is found
                    
                    // Check to see who won
                    if gameState[combination[0]] == 1 {
                        winner.text = "Cross has won"
                    } else {
                        winner.text = "Nought has won"
                    }
                    winner.isHidden = false
                    playButton.isHidden = false
                }
            }
            
            // Its possible that all states have changed to 1 or 2 and no winner has been found so we also need to check if any 0s are left. In that case its a draw.
            
            gameIsActive = false // Automaticaly assume game has ended as a draw unless there are more states to be played.
            
            for state in gameState {
                if state == 0 {
                    gameIsActive = true
                    break
                }
            }
            
            if !gameIsActive {
                winner.text = "It's a draw"
                winner.isHidden = false
                playButton.isHidden = false
            }
            
            // Challenge: In case there is a winner by the last state we get a draw. How can we make sure we get the winner?
        }
    }
    
    @IBAction func playAgain(_ sender: UIButton) {
        
        winner.isHidden = true
        playButton.isHidden = true
        
        gameState = [0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ]
        gameIsActive = true
        activePlayer = 1
        
        resetImages()
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        winner.isHidden = true
        playButton.isHidden = true
        resetImages()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    func resetImages() {
        for button in view.subviews {
            if let button = button as? UIButton {
                button.setImage(nil, for: .normal)
            }
        }
    }

}


```
## チャレンジ問題

1. 最後の勝者からゲームをスタートするにはどうすればいいですか？

## お願い

もし次回以降にあなたが学びたいものがあれば教えてください。
