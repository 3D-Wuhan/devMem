# wrap vcglib with openGL


```cpp
vcglib\wrap\gl\gl_mesh_attributes_multi_viewer_bo_manager.h

/****************************************************WARNING!!!!!!!!!!!!!!!!!*********************************************************************************************/
//You must inherit from NotThreadSafeGLMeshAttributesMultiViewerBOManager, providing thread safe mechanisms, in order to use the bo facilities exposed by the class.
//In wrap/qt/qt_thread_safe_memory_rendering.h you will find a ready to use class based on QtConcurrency module.
/*************************************************************************************************************************************************************************/
template<typename MESH_TYPE, typename UNIQUE_VIEW_ID_TYPE = unsigned int, typename GL_OPTIONS_DERIVED_TYPE = RenderingModalityGLOptions>
class NotThreadSafeGLMeshAttributesMultiViewerBOManager : public GLMeshAttributesInfo
{
public:
	typedef PerViewData<GL_OPTIONS_DERIVED_TYPE> PVData;

protected:
	...
	void draw(UNIQUE_VIEW_ID_TYPE viewid, const std::vector<GLuint>& textid = std::vector<GLuint>()) const
	{
		typename ViewsMap::const_iterator it = _perviewreqatts.find(viewid);
		if (it == _perviewreqatts.end())
			return;

		const PVData& dt = it->second;
		//const InternalRendAtts& atts = it->second._intatts;
		drawFun(dt, textid);
	}
	...

private:
	void drawFun( const PVData& dt, const std::vector<GLuint>& textid = std::vector<GLuint>( ) ) const
	{
		glPushAttrib(GL_ALL_ATTRIB_BITS);
		glMatrixMode(GL_MODELVIEW);
		glPushMatrix();
		glMultMatrix(_tr);

		if ((dt._glopts != NULL) && (dt._glopts->_perbbox_enabled))
			drawBBox(dt._glopts);

		if (dt.isPrimitiveActive(PR_SOLID))
		{
			bool somethingmore = dt.isPrimitiveActive(PR_WIREFRAME_EDGES) || dt.isPrimitiveActive(PR_WIREFRAME_TRIANGLES) || dt.isPrimitiveActive(PR_POINTS);
			if (somethingmore)
			{
				glEnable(GL_POLYGON_OFFSET_FILL);
				glPolygonOffset(1.0, 1);
			}
			drawFilledTriangles(dt._intatts[size_t(PR_SOLID)], dt._glopts, textid);
			if (somethingmore)
				glDisable(GL_POLYGON_OFFSET_FILL);
		}

		if (dt.isPrimitiveActive(PR_WIREFRAME_EDGES) || dt.isPrimitiveActive(PR_WIREFRAME_TRIANGLES))
		{
			//InternalRendAtts tmpatts = atts;
			bool pointstoo = dt.isPrimitiveActive(PR_POINTS);

			if (pointstoo)
			{
				glEnable(GL_POLYGON_OFFSET_FILL);
				glPolygonOffset(1.0, 1);
			}
			bool solidtoo = dt.isPrimitiveActive(PR_SOLID);

			/*EDGE    |     TRI     |    DRAW
			---------------------------------
				TRUE         TRUE         EDGE
				TRUE         FALSE        EDGE
				FALSE        TRUE         TRI
				FALSE        FALSE        NOTHING */

			if (dt.isPrimitiveActive(PR_WIREFRAME_EDGES))
				drawEdges(dt._intatts[size_t(PR_WIREFRAME_EDGES)], dt._glopts);
			else
			{
				if (dt.isPrimitiveActive(PR_WIREFRAME_TRIANGLES))
				{
					drawWiredTriangles(dt._intatts[size_t(PR_WIREFRAME_TRIANGLES)], dt._glopts, textid);
				}
			}

			if (pointstoo || solidtoo)
				glDisable(GL_POLYGON_OFFSET_FILL);
		}
		if (dt.isPrimitiveActive(PR_POINTS))
			drawPoints(dt._intatts[size_t(PR_POINTS)], dt._glopts, textid);

		glPopMatrix();
		glPopAttrib();
		glFlush();
		glFinish();
	}

}
```

上述这个 drawFun 是最终进行绘制需要调用的函数，当然会被封装成线程安全后才能使用。
