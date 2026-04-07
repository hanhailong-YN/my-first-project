# my-first-project
##我的第一个项目，今天学会了用VS code操作GitHub!
#!/bin/bash
IFS='|' read -ra args <<< "1860811275621797251"
for arg in "${args[@]}"; do
  echo "处理参数: $arg"
        bazel run -c opt //experimental/users/chuanyang:download_run  -- --run_name=$arg  --download_camera=true

done

[sudoku_game.py](https://github.com/user-attachments/files/26528676/sudoku_game.py)
import random

# 1. 打印数独棋盘（格式化输出，清晰区分3x3宫格）
def print_sudoku(board):
    for i in range(9):
        # 每3行打印一条水平分隔线（除了第0行）
        if i % 3 == 0 and i != 0:
            print("-" * 21)
        for j in range(9):
            # 每3列打印一条垂直分隔线（除了第0列）
            if j % 3 == 0 and j != 0:
                print("|", end=" ")
            # 打印数字，0表示空缺位置，显示为空格
            if board[i][j] == 0:
                print(" ", end=" ")
            else:
                print(board[i][j], end=" ")
        print()  # 换行

# 2. 验证数字填入是否合法（核心：行、列、3x3宫格无重复）
def is_valid(board, row, col, num):
    # 验证当前行是否有重复数字
    for j in range(9):
        if board[row][j] == num:
            return False
    # 验证当前列是否有重复数字
    for i in range(9):
        if board[i][col] == num:
            return False
    # 验证当前3x3宫格是否有重复数字
    # 计算宫格的起始行和起始列（都是3的倍数）
    start_row = (row // 3) * 3
    start_col = (col // 3) * 3
    for i in range(3):
        for j in range(3):
            if board[start_row + i][start_col + j] == num:
                return False
    # 所有条件都满足，返回合法
    return True

# 3. 回溯法生成完整的合法数独棋盘
def fill_board(board):
    for i in range(9):
        for j in range(9):
            if board[i][j] == 0:
                # 随机打乱1-9的数字顺序，生成不同的棋盘
                nums = list(range(1, 10))
                random.shuffle(nums)
                for num in nums:
                    if is_valid(board, i, j, num):
                        board[i][j] = num
                        # 递归填充下一个空位，填充成功则返回True
                        if fill_board(board):
                            return True
                        # 回溯：当前数字无法填充后续空位，重置为0
                        board[i][j] = 0
                # 1-9都无法填充当前空位，返回False
                return False
    # 棋盘填满，返回True
    return True

# 4. 挖空生成游戏棋盘（挖空数量越多，难度越高）
def generate_sudoku(difficulty=40):
    # 初始化空棋盘
    board = [[0 for _ in range(9)] for _ in range(9)]
    # 填充完整棋盘
    fill_board(board)
    # 挖空：随机选择位置置为0，挖空数量由difficulty控制
    holes = 0
    while holes < difficulty:
        row = random.randint(0, 8)
        col = random.randint(0, 8)
        if board[row][col] != 0:
            board[row][col] = 0
            holes += 1
    return board

# 5. 检查棋盘是否已填满（游戏胜利条件）
def is_complete(board):
    for i in range(9):
        for j in range(9):
            if board[i][j] == 0:
                return False
    return True

# 6. 主游戏流程
def sudoku_game():
    print("=" * 30)
    print("    Python 数独小游戏")
    print("=" * 30)
    print("游戏规则：")
    print("1. 棋盘是9x9网格，分为9个3x3宫格")
    print("2. 需在空缺位置（空格）填入1-9的数字")
    print("3. 每行、每列、每个3x3宫格内数字不能重复")
    print("4. 输入格式：行 列 数字（用空格分隔，行/列从0开始计数）")
    print("5. 输入 'quit' 可退出游戏")
    print("=" * 30)

    # 选择难度（控制挖空数量）
    while True:
        try:
            diff = input("请选择难度（1-简单 2-中等 3-困难）：")
            if diff == "1":
                board = generate_sudoku(30)  # 简单：30个空位
                break
            elif diff == "2":
                board = generate_sudoku(40)  # 中等：40个空位
                break
            elif diff == "3":
                board = generate_sudoku(50)  # 困难：50个空位
                break
            else:
                print("输入无效，请输入1、2或3！")
        except:
            print("输入无效，请输入1、2或3！")

    # 游戏循环
    while True:
        print("\n当前数独棋盘：")
        print_sudoku(board)

        # 检查是否游戏胜利
        if is_complete(board):
            print("🎉 恭喜你！成功完成数独！")
            break

        # 获取用户输入
        user_input = input("\n请输入（行 列 数字）或输入quit退出：").strip()
        if user_input.lower() == "quit":
            print("👋 游戏退出，欢迎下次再来！")
            break

        # 解析用户输入
        try:
            parts = user_input.split()
            if len(parts) != 3:
                print("❌ 输入格式错误！请按“行 列 数字”格式输入（用空格分隔）")
                continue
            # 转换为整数（行、列、数字）
            row = int(parts[0])
            col = int(parts[1])
            num = int(parts[2])

            # 验证行、列范围
            if row < 0 or row > 8 or col < 0 or col > 8:
                print("❌ 行和列必须是0-8之间的整数！")
                continue
            # 验证数字范围
            if num < 1 or num > 9:
                print("❌ 数字必须是1-9之间的整数！")
                continue
            # 验证当前位置是否已有数字
            if board[row][col] != 0:
                print(f"❌ 该位置（{row},{col}）已有数字，无法覆盖！")
                continue
            # 验证填入数字是否合法
            if not is_valid(board, row, col, num):
                print(f"❌ 数字{num}填入（{row},{col}）不合法（行/列/宫格重复）！")
                continue

            # 所有验证通过，填入数字
            board[row][col] = num
            print(f"✅ 成功在（{row},{col}）填入数字{num}！")

        except ValueError:
            print("❌ 输入无效！请输入整数，格式为“行 列 数字”")
        except Exception as e:
            print(f"❌ 程序异常：{e}，请重新输入！")

# 启动游戏
if __name__ == "__main__":
    sudoku_game()
