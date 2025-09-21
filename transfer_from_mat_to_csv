 import os
import scipy.io
import pandas as pd
import numpy as np

def mat_to_df(mat_file):
    mat_data = scipy.io.loadmat(mat_file)
    data = {k: v for k, v in mat_data.items() if not k.startswith("__")}
    if not data:
        return None

    dfs = []
    for key, value in data.items():
        arr = np.array(value)

        # 保证至少二维
        if arr.ndim == 1:
            arr = arr.reshape(-1, 1)

        # 多列时，列名加索引
        cols = [f"{key}_{i}" if arr.shape[1] > 1 else key for i in range(arr.shape[1])]
        df = pd.DataFrame(arr, columns=cols)
        dfs.append(df)

    # 横向拼接
    final_df = pd.concat(dfs, axis=1)
    return final_df


def convert_folder(input_folder, output_folder):
    for root, _, files in os.walk(input_folder):
        for file in files:
            if file.endswith(".mat"):
                mat_path = os.path.join(root, file)
                df = mat_to_df(mat_path)
                if df is not None:
                    # 计算输出路径
                    rel_path = os.path.relpath(root, input_folder)
                    save_dir = os.path.join(output_folder, rel_path)
                    os.makedirs(save_dir, exist_ok=True)
                    save_path = os.path.join(save_dir, file.replace(".mat", ".csv"))
                    
                    print(f"正在保存: {save_path}")
                    df.to_csv(save_path, index=False, encoding="utf-8-sig")


if __name__ == "__main__":
    input_folder = "源域数据集"
    output_folder = "csv数据集"
    convert_folder(input_folder, output_folder)
