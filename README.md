
# SWITCHER GENÉRICO PARA CPT

Para facilitar a administração do CPT pelo Painel do Wordpress.

Vantagens:

+ Rápido
+ Prático
+ Não precisa instalar Plugin
+ Não precisa saber programar
+ Ícones com 2k cada: <img src="/switcher-true.gif"> <img src="/switcher-false.gif">


## Procedimentos

1) Faça download das imagens "switcher-true.gif" e "switcher-false.gif";
2) Faça upload das imagens no Painel do Wordpress em "Mídia";
3) Use o gerenciador de arquivos (CPANEL) para editar o arquivo functions.php localizado em: theme/nome do thema/functions.php;
4) Copie o código "Switcher Genérico" e cole após o último código da página funcions.php;
5) Altere as varíaveis: CAMINHO-DO-ARQUIVO, SLUG-CPT, SLUG-SWITCHER;
6) Salve o funcions.php, vá para o Painel do Wordpress, clique no CPT para abrir e faça os teste clicando nos switchers.


## Instalação / Código Switcher


```bash
  //===================================================================
//							SWITCHER
//===================================================================
//  Cria um switcher no CPT GENÉRICO COM META FIELD GENÉRICO
// 	Alterar: CAMINHO-DO-ARQUIVO, SLUG-CPT, SLUG-SWITCHER

function add_status_column_to_cpt($cpt, $meta_field) {
    add_filter('manage_' . $cpt . '_posts_columns', function($columns) use ($meta_field) {
        $columns[$meta_field] = 'Status';
        return $columns;
    });

    add_action('manage_' . $cpt . '_posts_custom_column', function($column_name, $post_id) use ($meta_field) {
        if ($column_name == $meta_field) {
            $meta = get_post_meta($post_id, $meta_field, true);
            if ($meta == '1' || $meta == 'true') {
                echo '<img src="CAMINHO-DO-ARQUIVO/switcher-true.gif" data-post-id="' . $post_id . '" class="status-switcher" style="cursor:pointer" data-meta-field="' . $meta_field . '" />';
            } else {
                echo '<img src="CAMINHO-DO-ARQUIVO/switcher-false.gif" data-post-id="' . $post_id . '" class="status-switcher" style="cursor:pointer" data-meta-field="' . $meta_field . '" />';
            }
        }
    }, 10, 2);
}

add_action('admin_footer', function() {
    global $pagenow;
    if ($pagenow == 'edit.php') {
        ?>
        <script>
            jQuery(document).ready(function($) {
                let variavel = '<?php echo $pagenow;?>';
                //console.log('variavel: '+variavel);
                $('.status-switcher').on('click', function() {
                    var post_id = $(this).data('post-id');
                    var meta_field = $(this).data('meta-field');
                    var status = $(this).attr('src') == 'CAMINHO-DO-ARQUIVO/switcher-true.gif' ? 'false' : 'true';
                    var img = $(this);
                    if (status == '1' || status == 'true') {
						img.attr('src', 'CAMINHO-DO-ARQUIVO/switcher-true.gif').css({'cursor':'pointer'});
					} else {
						img.attr('src', 'CAMINHO-DO-ARQUIVO/switcher-false.gif').css({'cursor':'pointer'});
					}
                    $.post(ajaxurl, {
                        action: 'update_status',
                        post_id: post_id,
                        status: status,
                        meta_field: meta_field
                    });
                });
            });
        </script>
        <?php
    }
});

add_action('wp_ajax_update_status', function() {
    $post_id = $_POST['post_id'];
    $status = $_POST['status'];
    update_post_meta($post_id, $_POST['meta_field'], $status);
    wp_die();
});

// ESTE CÓDIGO É QUE FACILITA TUDO
add_status_column_to_cpt('SLUG-CPT', 'SLUG-SWITCHER');
```
    
## Autor

- [@wilsonrg](https://github.com/wilsonrg)

