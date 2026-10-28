# Visuais para Ragnarok (rAthena)

Pacote com **2.623 visuais** (itens da aba costume), prontos para colocar num servidor rAthena:

- **uma loja** com todos eles, separados por posição (topo, meio, baixo, capa e combinados);
- **33 visuais exclusivos do Ragnarok LATAM**, que não existem no kRO;
- as **imagens do cliente** (sprites e ícones) dos visuais que faltam no `data.grf` do kRO, num único GRF de 31 MB;
- os **nomes e descrições** em português dos 182 visuais que o cliente kRO padrão não conhece.

## Requisitos

- **Servidor:** rAthena Renovação recente (2025 ou mais novo).
- **Cliente:** 2020 ou mais novo, com a aba de costume e `data.grf` do kRO atualizado.

## O que tem aqui

| Arquivo | Para quê |
|---|---|
| `servidor/npc/lojas_visuais.txt` | NPC "Loja de Visuais" com menu para as 8 lojas |
| `servidor/db/import/item_db_costumes_latam.yml` | Os 33 visuais do LATAM no banco de itens |
| `cliente/visuais.grf` | Sprites, ícones e tabelas de visual (`accessoryid`, `accname`, `spriterobeid`, `spriterobename`) |
| `cliente/SystemEN/itemInfo_Visuais.lua` | Nomes e descrições dos visuais que o cliente kRO não tem |

## Passo 1: servidor

1. **Loja.** Copie `servidor/npc/lojas_visuais.txt` para `npc/custom/visuais/lojas_visuais.txt` e acrescente esta linha no fim do `npc/scripts_custom.conf`:

   ```
   npc: npc/custom/visuais/lojas_visuais.txt
   ```

   - O NPC fica em **Prontera 139,163**. Para mudar, edite a linha `prontera,139,163,4` no começo do arquivo.
   - O preço é **10.000 zeny** por visual. Para mudar, edite `.preco = 10000;` no `OnInit`.

2. **Visuais do LATAM.** Copie `servidor/db/import/item_db_costumes_latam.yml` para `db/import/` e importe o arquivo no final do seu `db/import/item_db.yml`:

   ```yaml
   Footer:
     Imports:
       - Path: db/import/item_db_costumes_latam.yml
   ```

   Se o seu `item_db.yml` já tiver um `Footer`, acrescente só a linha do `- Path`.
   Antes, confira se nenhum dos IDs do arquivo já está em uso no seu servidor.

3. **Reinicie o map-server** e confira o log. Se algum visual da loja não existir no seu `item_db`, ele aparece como erro no log e fica fora da loja; o resto funciona normalmente.

## Passo 2: cliente

Feche o jogo antes, porque o cliente trava os arquivos GRF.

1. **GRF.** Copie `cliente/visuais.grf` para a pasta do cliente e coloque-o **em primeiro** no `DATA.INI`, empurrando os outros uma posição para baixo:

   ```ini
   [Data]
   0=visuais.grf
   1=seu_servidor.grf
   2=data.grf
   ```

   O `visuais.grf` traz as tabelas de visual completas do kRO de out/2026 mais os 33 do LATAM. **Se o seu servidor já tem chapéus customizados** nessas tabelas, coloque o seu GRF acima do `visuais.grf` e junte as duas versões das tabelas. Elas ficam em `data\luafiles514\lua files\datainfo\` e estão em texto puro; dá para abrir com o GRF Editor.

2. **Nomes e descrições.**
   - **Cliente com ROenglishRE** (pasta `SystemEN` com multi-itemInfo): copie `cliente/SystemEN/itemInfo_Visuais.lua` para a pasta `SystemEN` (ou `System`, conforme o seu cliente) e acrescente no seu `itemInfo.lua`:

     ```lua
     ImportFiles = {
         -- (os que já existem)
         "itemInfo_Visuais.lua",
     }
     ImportTables = {
         -- (os que já existem)
         "visuais",
     }
     ```

   - **Outros clientes:** copie as entradas de dentro de `tbl_visuais = { ... }` para dentro da tabela do seu `itemInfo`. O formato de cada entrada é o padrão (`identifiedDisplayName`, `ClassNum` e os outros campos).

   Os textos estão em português e o arquivo está em Windows-1252, que é o que o cliente em inglês lê.

## Para testar

1. Entre no jogo e fale com a **Loja de Visuais** em Prontera 139,163.
2. Compre um visual de cada loja e equipe na aba de costume.
3. Se aparecer **"Resource File Loading fail"** ou o personagem ficar sem o visual, confira se o `visuais.grf` está em primeiro no `DATA.INI`.

## Créditos

- Sprites, ícones e textos são do Ragnarok Online (Gravity), do cliente kRO e do Ragnarok LATAM.
- O formato multi-itemInfo é do [ROenglishRE](https://github.com/llchrisll/ROenglishRE).
