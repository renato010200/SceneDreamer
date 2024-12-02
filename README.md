# SceneDreamer
O SceneDreamer aprende a gerar cenas 3D ilimitadas a partir de coleções de imagens 2D existentes.

Este método pode sintetizar diversas paisagens em diferentes estilos, com consistência 3D, profundidade bem definida e trajetória de câmera livre.

# Instalação
É recomendado fortemente usar o Anaconda para gerenciar seu ambiente python. Você pode configurar o ambiente necessário pelos seguintes comandos:


## Instalação das Dependências do Python

1. Crie o ambiente Conda usando o arquivo `environment.yaml`:

    ```bash
    conda env create -f environment.yaml
    ```

2. Ative o ambiente:

    ```bash
    conda activate scenedreamer
    ```

## Compilação das Bibliotecas de Terceiros

1. Determine a versão do CUDA instalada:

    ```bash
    export CUDA_VERSION=$(nvcc --version | grep -Po "(\d+\.)+\d+" | head -1)
    ```

2. Compile as bibliotecas de terceiros necessárias. Execute o seguinte para cada diretório (`correlation`, `channelnorm`, `resample2d`, `bias_act`, `upfirdn2d`):

    ```bash
    CURRENT=$(pwd)
    for p in correlation channelnorm resample2d bias_act upfirdn2d; do
        cd imaginaire/third_party/${p};
        rm -rf build dist *info;
        python setup.py install;
        cd ${CURRENT};
    done
    ```

3. Para a biblioteca `voxlib` no diretório `gancraft/`, execute:

    ```bash
    for p in gancraft/voxlib; do
      cd imaginaire/model_utils/${p};
      make all;
      cd ${CURRENT};
    done
    ```

## Compilação do GridEncoder

1. Navegue até o diretório `gridencoder` e construa as extensões:

    ```bash
    cd gridencoder
    python setup.py build_ext --inplace
    ```

2. Instale o pacote:

    ```bash
    python -m pip install .
    ```

3. Volte para o diretório principal:

    ```bash
    cd ${CURRENT}
    ```


# Renderizar!
Você pode executar o seguinte comando para gerar seu próprio mundo 3D!

python inference.py --config configs/scenedreamer_inference.yaml --output_dir ./test/ --seed 8888 --checkpoint ./scenedreamer_released.pt

# Exemplos 

Aqui está um exemplo de uma imagem em 3D
![sample_traj](https://github.com/user-attachments/assets/8bef1cf5-9c6c-48f3-858d-19b8610705e4)

# Obrigado!

