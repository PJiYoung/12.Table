# 12. Table
## 테이블 뷰 컨트롤러 이용하여 할 일 목록


이 앱을 만들기 위해서는 목록을 삭제하는 함수도 필요하고 목록 순서 바꾸는 코드와 새 목록을 추가하는 함수 등이 필요한데 이 코드를 밑에서 설명할 예정이다.

**앱 시작시 기본적으로 나타낼 목록란에는 책 구매, 철수와 약속, 스터디 준비하기를 나타내야하는데 이를 나타내기 위해선 첫 번째에 이 코드를 작성하면 된다.**
'''
      var items = ["책 구매", "철수와 약속", "스터디 준비하기"]
      var itemsImageFile = ["cart.png", "clook.png", "pencil.png"]
'''
외부 변수인 items의 내용을 각각 지정한 후, 외부 변수인 이미지 파일을 알맞게 각각 지정한다.

      override func viewWillAppear(_ animated: Bool) {
        tvListView.reloadData()
      }
위에 코드는 뷰가 노출될 때마다 리스트의 데이터를 다시 불러온다.
  
'''
       override func numberOfSections(in tableView: UITableView) -> int {
          return 1
       } // 테이블 안의 섹션 개수를 1로 설정하기 위해선 이 코드를 사용하면 된다.
    
        override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> int {
         return items.count
         } // 이 코드는 섹션당 열의 개수를 전달해준다.
 '''
 
  **목록을 삭제하는 함수는**
 '''
      override func tableView(_ tableView: UITableView, commitEfitongStyle
              editingStyle: UITableViewCell.EditingStyle, forRowAtIndexPath indaxPath:
              IndexPath) {
           if editingStyle ==.delete {
      
             //items와 itemsImageFile에서 해당 리스트를 삭제한다.
              items.remove(at: (IndexPath as NsIndexPath).row)
              itemsImageFile.remove(at: (indexPath as NsIndexPath).row)
             tableView.deleteRows(at: [indexPath], with: .fade)
          } else if editingStyle == .insert {
             // Create a new instance of the appropriate class, insert it into
             the array, and add a new row to the table view
           }
       }
'''       
이 코드에서 삭제과정이 이루어 진다.
   
삭제 시 "Delete" 대신 " 삭제" 로 표시되게 하려고 하는데 그럴경우
'''
        override func tableView(_ tableView: UITableView, titleForDeleteConfirmation
                    ButtonForRowAt indexPath: IndexPath)
                        -> String? {
                    return "삭제"
         }
'''
이렇게 적으면 바뀌게 된다.
   
**목록의 순서를 바꾸게 할 경우 함수 안에 하나의목록을 선택하여 다른 곳으로 이동하는 소스를 추가한다. 먼저 변수를 만들어서 이동할 변수를 기억한 후 이동할 목록을 삭제하고 변수에 저장된 내용을 이동한 곳으로 삽입한다.**
'''
      override func tableView(_ tableView: UITableView, moveRowAt
         fromIndexPath: IndexPath, to: IndexPath) {
         let itemToMove = items[(fromIndexPath as NSIndexPath).row]
         let itemImageToMove = itemsImageFile[(fromIndexPath as NSIndexPath).row]
         items.remove(at: (fromIndexPath as NSIndexPath).row)
         itemsImageFile.remove(at: (fromIndexPath as NSIndexPath).row)
         items.insert(itemToMove, at: (to as NSIndexPath).row)
         itemsImageFile.insert(itemsImageToMove, at: (to as NSIndexPath).row)
    }
 '''
 위의 코드가 되는 방법을 let부분부터 작성하자면 이동할 아이템의 위치를 itemToMovw에 저장하고 이동할 아이템의 이미지를 itemImageToMove에 저장한다. 이동할 아이템을 삭제하고 이때 삭제한 아이템 뒤의 아이템들의 인덱스가 재정렬된다. 4번째 줄에선 이동할 아이템의 이미지를 삭제하고 이때 삭제한 아이템 이미지 뒤의 아이템 이미지들의 인덱스가 재정렬된다. 그다음 삭제된 아이템을 이동할 위치로 삽입하고 또한 삽입한 아이템 뒤의 아이템들의 인덱스가 재정렬된다. 이 코드의 마지막 줄은 삭제된 아이템의 이미지를 이동할 위치로 삽입하고 또한 삽입한 아이템 이미지 뒤의 아이템 이미지들의 인덱스가 재정렬되게 된다.
 
 이렇게까지 코드를 작성하였을때 앱을 실행하게 된다면 목록 오른쪽에 있던 버튼이 '순서 바꾸기'버튼으로 바뀌어 있는 것을 확인할 수 있고, 옮기고 싶은 목록을 끌어다 놓고 [Done]버튼을 클릭해 저장하면 된다.
 
**새 목록을 추가하는 코드는**
'''
    @IBAction func btnAddItem(_ sender: UIButton) {
       items.append(tfAddItem.text!)
       itemsImageFile.append("clock.png")
       tfAddItem.text=""
       _= navigationController?.popViewController(animated: true)
    }
 '''
를 작성하면 되는데 각 소스의 의미를 작성하자면 items에 텍스트 필드의 텍스트 값을 추가한다. itemsImageFile에는 무조건 'clock.png'파일을 추가하는 소스이고 세번째 줄은  텍스트 필드의 내용을 지우는 소스이다. 마지막 줄은 루트 뷰 컨트롤러, 즉 테이블 뷰로 돌아가라는 소스이다.
 
 모든 코드를 작성한 후 결과를 보게된다면 [실행]버튼을 클릭한 후 목록 중에서 하나를 클릭하면 'Detail View'로 전환하며 내용이 뜨게 된다.
 
 이 앱은 'Main View'에 시작할때 기본적으로 나타나는 목록들이 나오고 그 목록을 클릭할 경우 "Detail View'로 화면이 전환되며 목록 이름이 뜨게 된다.
    
  
  
