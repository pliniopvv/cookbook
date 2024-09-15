# Incluir bibliotecas "manualmente" no Visual Studio 2022 em projetos C/C++

Vá em:
 - Project
 - Properties
 - VC++ Directories e adicione:
   - "Include Directories" => Diretório onde está os headerfiles adicionados.
       - Por exemplo: "$(ProjectDir)include"
   - "Library Directories" => Diretório onde estão os arquivos .lib adicionados
       - Por exemplo: "$(ProjectDir)lib"
 - Linker
 - Input e adicione:
   - as bibliotecas que serão utilizadas no projeto.

Realizar os includes com "<>" e basta compilar e colocar as .dll's no mesmo diretório que o executável.